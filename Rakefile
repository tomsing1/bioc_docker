require 'erb'
require 'ostruct'
require 'yaml'
require 'fileutils'
require 'pp'
require 'rake'

require 'docker'
require 'parallel'

class ErbBinding < OpenStruct
    def get_binding
        return binding()
    end
end

desc "create Dockerfiles from templated input (default)"
task :build  do
    basedir = File.dirname(__FILE__) + File::SEPARATOR
    config = YAML.load_file(basedir + File::SEPARATOR + "config.yml")
    for key in config.keys
        hsh = config[key]
        common_dir = basedir + "common"
        source_dir = basedir +  hsh['source_dir']
        out_dir = basedir + hsh['out_dir'].gsub("/", File::SEPARATOR)
        unless File.exists? out_dir
            FileUtils::mkdir_p out_dir
        end
        data = hsh['data']
        infiles = Dir.new(source_dir).entries.reject{|i| i == "." or i == ".."}
        infiles = infiles.map{|i| source_dir + File::SEPARATOR + i}
        common_files = Dir.new(common_dir).entries.reject{|i| i == "." or i == ".."}
        common_files = common_files.map{|i| common_dir + File::SEPARATOR + i}

        for file in infiles+common_files
            if file.end_with? ".in"
                vars = ErbBinding.new(data)
                erb = ERB.new(File.new(file).read, nil, "%")
                vars_binding = vars.send(:get_binding)
                out_filename = out_dir + File::SEPARATOR + File.basename(file).sub(/\.in$/, "")
                out_fh = File.open(out_filename, "w")
                out_fh.write(erb.result(vars_binding)) # FIXME only do this if file has changed
                out_fh.close()
            else
                # FIXME only do this if files differ
                FileUtils.cp(file, out_dir, :preserve => true)
            end
        end
    end
end

task :default => :build
