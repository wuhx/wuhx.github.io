task :default => :help

task :help do
  puts "1. install jekyll: rake install"
  puts "2. start jekyll  : rake run"
  puts "3. write a post  : rake new title=post-title"  
end

task :run do
  puts "jekyll running at http://127.0.0.1:4000"
  `bundle exec jekyll serve`
end

task :new do |t, args|
  if ENV['title'] then
    new_post(ENV['title'])
  else
    puts 'rake new title="post-title"'
  end
end

task :publish do
  puts "git push to remote repo"
  `jekyll`
  `git commit . -m 'update' && git push`
  `cd _site && git add . --all && git commit . -m 'update' && git push`
end



task :install do
  puts "bundle install jekyll --path vendor/bundle"
  `sudo gem install bundle`
  `bundle install --path vendor/bundle`
end

desc 'Build site with Jekyll'
task :generate => [:clean, :scss] do
  `jekyll`
end

desc 'Generate css'
task :scss do
  `scss media/css/style.scss media/css/style.css`
end

desc 'Start server'
task :server => [:clean, :scss] do
  `jekyll serve -t`
end

desc 'Deploy with rake "depoly[comment]"'
task :deploy, [:comment] => :generate do |t, args|
  if args.comment then
    `git commit . -m '#{args.comment}' && git push`
    `cd _site && git commit . -m '#{args.comment}' && git push`
  else
    `git commit . -m 'update' && git push`
    `cd _site && git commit . -m 'update' && git push`
  end
end

desc 'Clean up'
task :clean do
  `rm -rf _site`
end

def new_post(title)
  time = Time.now
  filename = "_posts/" + time.strftime("%Y-%m-%d-") + title + '.md'
  if File.exists? filename then
    puts "Post already exists: #{filename}"
    return
  end
  uuid = `uuidgen | tr "[:upper:]" "[:lower:]" | tr -d "\n"`
  File.open(filename, "wb") do |f|
    f << <<-EOS
---
title: #{title}
layout: post
tags:
---


EOS
  %x[echo "#{filename}" | pbcopy]
  end
  puts "created #{filename}"
  #`git add #{filename}`
  `open "#{filename}"`
end
