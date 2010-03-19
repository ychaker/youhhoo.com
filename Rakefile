desc "deploy site to whichrubycmsshouldiuse.com"
task :deploy do
  require 'rubygems'
  require 'highline/import'
  require 'net/ssh'

  branch = "master"

  username = ask("Username:  ") { |q| q.echo = true }
  password = ask("Password:  ") { |q| q.echo = "*" }

  Net::SSH.start('74.50.57.225', username, :port => 22, :password => password) do |ssh|
    commands = <<EOF
cd /var/www/whichrubycmsshouldiuse.com
git pull origin #{branch}
rm -R _site/
jekyll --no-auto
EOF
    commands = commands.gsub(/\n/, "; ")
    ssh.exec commands
  end
end


desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  html = ''
  html << <<-HTML
---
layout: default
title: Youssef Chaker - Tags
---
<div id='tags' class='white'>
  HTML
  
  site.tags.sort.each do |tag, posts|
    html << <<-HTML
    
    <h3 id="#{tag.gsub(/ /, '-')}">Postings tagged "#{tag}"</h3>
    HTML
    
    html << '<ul class="posts">'
    posts.each do |post|
      post_data = post.to_liquid
      html << <<-HTML
        <li><a href="#{post.url}">#{post_data['title']}</a></li>
      HTML
    end
    html << '</ul>'
  end
  
  html << '</div>'
  File.open("tags.html", 'w+') do |file|
    file.puts html
  end
  
  puts 'Done.'
end

desc 'Generate categories page'
task :categories do
  puts "Generating categories..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  html = ''
  html << <<-HTML
---
layout: default
title: Youssef Chaker - Categories
---
<div id='categories' class='white'>
  HTML
  
  site.categories.sort.each do |category, posts|
    html << <<-HTML
    
    <h3 id="#{category.gsub(/ /, '-')}">Postings in "#{category}"</h3>
    HTML
    
    html << '<ul class="posts">'
    posts.each do |post|
      post_data = post.to_liquid
      html << <<-HTML
        <li><a href="#{post.url}">#{post_data['title']}</a></li>
      HTML
    end
    html << '</ul>'
  end
  
  html << '</div>'
  File.open("categories.html", 'w+') do |file|
    file.puts html
  end
  
  puts 'Done.'
end

desc 'Generate tag cloud'
task :tagcloud do
  puts "Generating tag cloud..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  
  tags = site.tags
  from = 1
  unto = 6
  # http://github.com/henrik/henrik.nyh.se/blob/6f2d52fd2c44cf12aea14eb87087a73340a5e12c/_helpers.rb
  tag_counts = tags.map {|tag,posts| [tag, posts.length] }.sort_by {|tag, count| tag.downcase }
  min = tag_counts.min { |a,b| a.last <=> b.last }.last
  max = tag_counts.max { |a,b| a.last <=> b.last }.last
  tag_counts_sizes = tag_counts.map { |tag, count|
    # http://blogs.dekoh.com/dev/2007/10/29/choosing-a-good-font-size-variation-algorithm-for-your-tag-cloud/
    weight = (Math.log(count)-Math.log(min))/(Math.log(max)-Math.log(min))
    size = from + ((unto-from)*weight).round
    [tag, count, size]
  }
  
  tag_cloud = ['<ol id="tag-cloud">',
      tag_counts_sizes.map {|t,c,s|
        title = c == 1 ? "1 post" : "#{c} posts"
        %{<li class="tier-#{s}" title="#{title}"><a href="/tags.html\##{t.gsub(/ /, '-')}">#{t}</a></li>}
      }.join(' '),
    '</ol>'
  ].join
  
  html = ''
  html <<  <<-HTML
  <div id="tag-cloud">
    <h4>Tags:</h4>
  HTML
  
  html << tag_cloud
  
  html << '</div>'
  File.open("_includes/tag_cloud.html", 'w+') do |file|
    file.puts html
  end
  
  puts 'Done.'
end

desc 'Generate category cloud'
task :catcloud do
  puts "Generating category cloud..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  
  categories = site.categories
  from = 1
  unto = 6
  # http://github.com/henrik/henrik.nyh.se/blob/6f2d52fd2c44cf12aea14eb87087a73340a5e12c/_helpers.rb
  cat_counts = categories.map {|cat,posts| [cat, posts.length] }.sort_by {|cat, count| cat.downcase }
  min = cat_counts.min { |a,b| a.last <=> b.last }.last
  max = cat_counts.max { |a,b| a.last <=> b.last }.last
  cat_counts_sizes = cat_counts.map { |cat, count|
    # http://blogs.dekoh.com/dev/2007/10/29/choosing-a-good-font-size-variation-algorithm-for-your-tag-cloud/
    weight = (Math.log(count)-Math.log(min))/(Math.log(max)-Math.log(min))
    size = from + ((unto-from)*weight).round
    [cat, count, size]
  }
  
  cat_cloud = ['<ol id="cat-cloud">',
      cat_counts_sizes.map {|t,c,s|
        title = c == 1 ? "1 post" : "#{c} posts"
        %{<li class="tier-#{s}" title="#{title}"><a href="/categories.html\##{t.gsub(/ /, '-')}">#{t}</a></li>}
      }.join(' '),
    '</ol>'
  ].join
  
  html = ''
  html <<  <<-HTML
  <div id="cat-cloud">
    <h4>Categories:</h4>
  HTML
  
  html << cat_cloud
  
  html << '</div>'
  File.open("_includes/cat_cloud.html", 'w+') do |file|
    file.puts html
  end
  
  puts 'Done.'
end
