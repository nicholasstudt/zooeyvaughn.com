# Originally adopted from Raimonds Simanovskis Rakefile, 
#   http://github.com/rsim/blog.rayapps.com/blob/master/Rakefile
#   which was in turn adopted from Tate Johnson's Rakefile
#   http://github.com/tatey/tatey.com/blob/master/Rakefile

require "jekyll"
require "yaml"

$cwd = File.dirname(__FILE__)

task :default => :build

desc "Serve on Localhost with port 8080"
task :preview => :tags do
	sh "jekyll --server 8080 --auto"
end
 
desc 'Build site with Jekyll.'
task :build => :tags do
	puts "Compiling website..."
    options = Jekyll.configuration({'source' => $cwd})
	@site = Jekyll::Site.new(options)
	@site.process
end
 
desc 'Generate tags pages'
task :tags  => :tag_cloud do
  puts "Generating tags..."

  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({'source' => $cwd})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  # Remove tags directory before regenerating
  FileUtils.rm_rf($cwd + "/archive/tags")

  site.tags.sort.each do |tag, posts|
    html = <<-HTML
---
layout: default
title: "Archive of '#{tag}'"
syntax-highlighting: yes
---
  <h1 class="title">Archive of tag "#{tag}"</h1>
  <ul>
  {% for post in site.posts %}
    {% for tag in post.tags %}
      {%if tag == "#{tag}" %}
        {% assign test_month = post.date | date: "%B %Y" %}
        {% if current_month != test_month %}
          {% assign current_month = test_month %}
          </ul>
          <h2>{{ current_month }}</h2>
          <ul>
        {% endif %}
        <li>
          <a href="{{ post.url }}">{{ post.title }}</a>
          <span>{{ post.date | date: "%d %B" }}</span>
        </li>
      {%endif%}
    {%endfor%}
  {% endfor %}
  </ul>
HTML

    FileUtils.mkdir_p($cwd + "/archive/tags/#{tag}")
    File.open($cwd + "/archive/tags/#{tag}/index.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end


desc 'Generate tags pages'
task :tag_cloud do
  puts 'Generating tag cloud...'

  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({'source' => $cwd})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  html = ''
  max_count = site.tags.map{|t,p| p.count}.max
  site.tags.sort.each do |tag, posts|
    s = posts.count
    font_size = ((20 - 10.0*(max_count-s)/max_count)*2).to_i/2.0
    html << "<a href=\"/archive/tags/#{tag}\" title=\"Postings tagged #{tag}\" style=\"font-size: #{font_size}px; line-height:#{font_size}px\">#{tag}</a> "
  end
  File.open($cwd+'/_includes/tag_cloud.html', 'w+') do |file|
    file.puts html
  end
  puts 'Done.'
end
