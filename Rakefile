#!/usr/bin/env rake
# encoding: utf-8

require 'rake/testtask'
require 'rubocop/rake_task'

# Rubocop
desc 'Run Rubocop lint checks'
task :rubocop do
  RuboCop::RakeTask.new
end

# lint the project
desc 'Run robocop linter'
task lint: [:rubocop]

# run tests
task default: :test
Rake::TestTask.new do |t|
  t.libs << 'test'
  t.pattern = 'test/unit/*_test.rb'
  t.warning = true
  t.verbose = true
  t.ruby_opts = ['--dev'] if defined?(JRUBY_VERSION)
end

namespace :test do
  task :isolated do
    Dir.glob('test/unit/*_test.rb').all? do |file|
      sh(Gem.ruby, '-w', '-Ilib:test', file)
    end or fail 'Failures'
  end

  task :resources do
    tests = Dir['test/resource/*.rb']
    return if tests.empty?
    sh(Gem.ruby, 'test/docker.rb', *tests)
  end

  task :runner do
    concurrency = ENV['CONCURRENCY'] || 4
    path = File.join(File.dirname(__FILE__), 'test', 'runner')
    sh('sh', '-c', "cd #{path} && kitchen converge -c #{concurrency}")
  end
end