require "rubygems"
require "bundler/setup"
require "stringex"

# Variables
posts_dir       = "_posts"
new_post_ext    = "md"

#######################
# Working with Jekyll #
#######################

# rake build
desc "Build atomthem.es site"
task :build do
  puts "## Generating Site with Jekyll"
  system "jekyll build"
end

# rake watch
desc "Serve and watch atomthem.es"
task :watch do
  system "jekyll serve -w"
end

# rake preview
desc "Launch a preview of atomthem.es in the browser"
task :preview do
    port = 4000
  Thread.new do
    puts "Launching browser for preview..."
    sleep 5
    system("#{open_command} http://localhost:#{port}/")
  end
  Rake::Task[:watch].invoke
end

# usage rake new_post[my-new-post] or rake new_post['my new post']
desc "Create a new theme in #{posts_dir}"
task :new_theme, :title do |t, args|
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  filename = "#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: theme"
    post.puts "thumbnail: /public/thumbnails/"
    post.puts "title: #{title.gsub(/&/,'&amp;')}"
    post.puts "tags: "
    post.puts "author: "
    post.puts "author_url: "
    post.puts "package_url: "
    post.puts "package_name: "
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
    post.puts "---"
  end
end

# Get the "open" command for rake preview
def open_command
  if RbConfig::CONFIG["host_os"] =~ /mswin|mingw/
    "start"
  elsif RbConfig::CONFIG["host_os"] =~ /darwin/
    "open"
  else
    "xdg-open"
  end
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end
