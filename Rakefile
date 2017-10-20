#coding:utf-8
require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'hz2py'		##
require 'date'

SOURCE = "."
CONFIG = {
  'tasks' => File.join(SOURCE, "Tasks"),
  'task_ext' => "todo",
}

# Path configuration helper
module JB
  class Path
    SOURCE = "."
    Paths = {
      :tasks => "Tasks"
    }
    
    def self.base
      SOURCE
    end

    # build a path relative to configured path settings.
    def self.build(path, opts = {})
      opts[:root] ||= SOURCE
      path = "#{opts[:root]}/#{Paths[path.to_sym]}/#{opts[:node]}".split("/")
      path.compact!
      File.__send__ :join, path
    end
  
  end #Path
end #JB

# Usage: rake task title="A Title"  date="20120209"
desc "Begin a new task in #{CONFIG['tasks']}"
task :task do
  abort("rake aborted: '#{CONFIG['tasks']}' directory not found.") unless FileTest.directory?(CONFIG['tasks'])
  title = ENV["title"] || "新任务"
  slug = Hz2py.do(title.encode('utf-8'), :join_with => '-', :to_simplified => true)
  slug = slug.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    # date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    date      = (ENV['date'] ? Time.parse(ENV['date']) : Date.today).strftime('%Y%m%d')
    dateBegin = date
    dateEnd   = (DateTime.strptime(date,'%Y%m%d').next_day(6)).strftime('%Y%m%d')
  rescue Exception => e
    puts "Error - date format must be YYYYMMDD, please check you typed it correctly!"
    exit -1
  end


  ## 想要中文换英文的名称，则使用一下命令:
  # filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")

  ## 这个可以直接显示中文名称:
  filename = File.join(CONFIG['tasks'], "#{dateBegin}-#{dateEnd}-#{title}.#{CONFIG['task_ext']}")

  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

   mkdir_p File.dirname(filename)
  puts "Creating new task: #{filename}"
  open(filename, 'w') do |post|
    # ----------------------------------------------
    post.puts ""
    # ----------------------------------------------
  end
  ## system("rstudio #{filename}") # 新建文章后在编辑器中打开
  system("subl #{filename}") # 新建文章后在编辑器中打开
end # task :task
## =============================================================================


## =============================================================================
def ask(message, valid_options)
  if valid_options
    answer = get_stdin("#{message} #{valid_options.to_s.gsub(/"/, '').gsub(/, /,'/')} ") while !valid_options.include?(answer)
  else
    answer = get_stdin(message)
  end
  answer
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end

#Load custom rake scripts
Dir['_rake/*.rake'].each { |r| load r }
## =============================================================================
