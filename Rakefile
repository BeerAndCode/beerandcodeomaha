task :default => :build

desc 'Clean up generated site'
task :clean do
  cleanup
end

desc 'Test site output for Liquid template errors'
task :test => :build do
  errors = `grep --exclude Rakefile -R 'Liquid error:' _site`
  if errors.nil? || errors.empty?
    puts "No errors"
  else
    puts "Errors:"
    puts errors.inspect
    exit 1
  end
end

desc 'Build site with jekyll'
task :build => :clean do
  submodule('update')
  jekyll
end

desc 'Start server with --auto'
task :server => :clean do
  jekyll('--server --auto')
end

desc 'Build and deploy'
task :deploy => :build do
  user = 'nicknisi'
  host = 'nicknisi.com'
  directory = '~/nicknisi.com'
  sh = "rsync -rtzh --progress --delete _site/ #{user}@#{host}:#{directory}"
end

desc 'new post'
task :post, [:name] do |t, args|
  if args.name
    template(argsr.name)
  else
    puts "Name required"
  end
end

def template(name)
  t = Time.now
  contents = ""
  File.open("_posts/yyyy-mm-dd-teplate.md", "rb") do |f|
    contents = f.read
  end
  contents = contents.sub("%date%", t.strftime("%Y-%m-%d %H:%M:%S %z")).sub("%title%", name)
  filename = "_posts/" + t.strftime("%Y-%m-%d") + name.downcase.gsub(/[^a-zA-Z0-9_\.]/, '-') + '.md'
  if File.exists? filename
    puts "Post already exists: #{filename}"
    return
  end
  File.open(filename, "wb") do |f|
    f.write contents
  end
  puts "created #{filename}"
end

def cleanup
  sh 'rm -rf _site'
end

def jekyll(opts = '')
  sh 'bundle exec jekyll ' + opts
end

def submodule(opts = '')
  sh 'git submodule ' + opts
end
