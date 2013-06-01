# Standard library
require 'rake'
require 'yaml'
require 'fileutils'

# Load the configuration file
config = YAML.load_file("_config.yml")

# Set "rake watch" as default task
task :default => :watch

# rake post["Post title"]
desc "Create a post in _posts"
task :post, :title do |t, args|
  title     = args[:title]
  template  = config["post"]["template"]
  extension = config["post"]["extension"]
  editor    = config["editor"]

  if title.nil? or title.empty?
    raise "Please add a title to your post."
  end

  date     = Time.now.strftime("%Y-%m-%d")
  filename = "#{date}-#{title.gsub(/(\'|\!|\?|\:|\s\z)/,"").gsub(/\s/,"-").downcase}.#{extension}"
  content  = File.read(template)

  if File.exists?("_posts/#{filename}")
    raise "The post already exists."
  else
    parsed_content = "#{content.sub("title:", "title: \"#{title}\"")}"
    File.write("_posts/#{filename}", parsed_content)
    puts "#{filename} was created."

    if editor && !editor.nil?
      sleep 2
      system "#{editor} _posts/#{filename}"
    end
  end
end

# rake draft["Post title"]
desc "Create a post in _drafts"
task :draft, :title do |t, args|
  title     = args[:title]
  template  = config["post"]["template"]
  extension = config["post"]["extension"]
  editor    = config["editor"]

  if title.nil? or title.empty?
    raise "Please add a title to your post."
  end

  filename = "#{title.gsub(/(\'|\!|\?|\:|\s\z)/,"").gsub(/\s/,"-").downcase}.#{extension}"
  content  = File.read(template)

  if File.exists?("_drafts/#{filename}")
    raise "The post already exists."
  else
    parsed_content = "#{content.sub("title:", "title: \"#{title}\"")}"
    File.write("_drafts/#{filename}", parsed_content)
    puts "#{filename} was created."

    if editor && !editor.nil?
      sleep 2
      system "#{editor} _drafts/#{filename}"
    end
  end
end

# rake publish
# rake publish["post-title"]
desc "Move a post from _drafts to _posts"
task :publish, :post do |t, args|
  post      = args[:post]
  extension = config["post"]["extension"]

  if post.nil? or post.empty?
    Dir["_drafts/*.#{extension}"].each do |filename|
      list = File.basename(filename, ".*")
      puts list
    end
  else
    date     = Time.now.strftime("%Y-%m-%d")
    filename = "#{post}.#{extension}"

    FileUtils.mv("_drafts/#{filename}", "_posts/#{date}-#{filename}")
    puts "#{filename} was moved to _posts."
  end
end

# rake build
desc "Generate the site (no server)"
task :build do
  system "jekyll --no-server"
end

# rake watch
# rake watch[number]
desc "Generate and watch the site (with an optional post limit)"
task :watch, :number do |t, args|
  number = args[:number]

  if number.nil? or number.empty?
    system "jekyll --auto --server"
  else
    system "jekyll --auto --server --limit_posts=#{number}"
  end
end

# rake preview
desc "Launch a preview of the site in the browser"
task :preview do
  require 'Launchy'

  Thread.new do
    puts "Launching browser for preview..."
    sleep 2
    Launchy.open("http://localhost:4000/")
  end

  Rake::Task[:watch].invoke
end

