require 'json'
require 'fileutils'

=begin
TODO: host javadoc on github? Maybe with github pages?
https://help.github.com/categories/20/articles
http://stackoverflow.com/questions/3939595/making-javadocs-available-with-github-sonatype-maven-repo
There! http://danbernier.github.com/WordCram/javadoc
=end

desc "Copies a fresh WordCram library into your Processing environment. See build.json!"
task 'publish.local' => :bundleForProcessing do
  sketch_folder = build_properties['processing.sketchFolder']
  lib_folder = File.join(sketch_folder, 'libraries')
  wc_folder = File.join(lib_folder, 'WordCram')

  FileUtils.rm_rf(wc_folder)
  FileUtils.cp_r('build/p5lib/WordCram', File.join(lib_folder))
end

task 'publish.release' => :bundleForProcessing do

  # git checkout master, first? Warn if you're not on master?

  puts "Give us a quick summary for Google Code:"
  summary = STDIN.gets.chomp

  puts "...and the release number:"
  release_number = STDIN.gets.chomp

  #puts "git tagging..."
  #puts `git tag #{release_number} -m "Tagging the #{release_number} release"`

  zipfile = "build/wordcram.#{release_number}.zip"
  tarfile = "build/wordcram.#{release_number}.tar.gz"

  puts "zipping & tarring..."
  puts `zip -5Tr #{zipfile} build/p5lib/WordCram`
  puts `tar -cvz build/p5lib/WordCram > #{tarfile}`

  puts "uploading to github..."
  #puts `github-downloads create -u danbernier -r WordCram -f #{zipfile}`   #  -d "description"
  #puts `github-downloads create -u danbernier -r WordCram -f #{tarfile}`   #  -d "description"


  puts "uploading javadoc to github..."
  `git checkout gh-pages`
  `cp -r build/p5lib/WordCram/reference javadoc`
  `git commit -m "Updating javadoc for #{release_number} release."`
  `git push`
end


%w[bundleForProcessing clean compile makeReleaseBranch publish.daily test].each do |task_name|

  desc "Run ant task #{task_name}"
  task task_name.to_sym do
    puts `ant #{task_name}`
  end

end

task :default => :test

def build_properties
  @props ||= JSON(File.read('build.json'))
end