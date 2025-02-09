require 'date'
require 'rake/clean'
require 'rake/extensiontask'
require 'digest/md5'

task :default => [:test, :spec]
task :spec => [:compile]

# Gem Spec
gem_spec = Gem::Specification.load('greenmat.gemspec')

# Ruby Extension
Rake::ExtensionTask.new('greenmat', gem_spec)

# Packaging
require 'bundler/gem_tasks'

# Testing
require 'rake/testtask'

Rake::TestTask.new('test:unit') do |t|
  t.libs << 'lib'
  t.libs << 'test'
  t.pattern = 'test/*_test.rb'
  t.verbose = true
  t.warning = false
end

task 'test:unit' => :compile

desc 'Run conformance tests (MARKDOWN_TEST_VER=1.0.3)'
task 'test:conformance' => :compile do |t|
  script  = "#{pwd}/bin/greenmat"
  version = ENV['MARKDOWN_TEST_VER'] || '1.0.3'
  lib_dir = "#{pwd}/lib"

  chdir("test/MarkdownTest_#{version}") do
    sh "RUBYLIB=\"$RUBYLIB:#{lib_dir}\" ./MarkdownTest.pl --script='#{script}' --tidy"
  end
end

desc 'Run version 1.0 conformance suite'
task 'test:conformance:1.0' => :compile do |t|
  ENV['MARKDOWN_TEST_VER'] = '1.0'
  Rake::Task['test:conformance'].invoke
end

desc 'Run 1.0.3 conformance suite'
task 'test:conformance:1.0.3' => :compile do |t|
  ENV['MARKDOWN_TEST_VER'] = '1.0.3'
  Rake::Task['test:conformance'].invoke
end

desc 'Run unit and conformance tests'
task :test => %w[test:unit test:conformance]

desc 'Run benchmarks'
task :benchmark => :compile do |t|
  $:.unshift 'lib'
  load 'test/benchmark.rb'
end

Dir['tasks/*.rake'].each { |file| load file }
