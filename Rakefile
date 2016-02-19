RIAK_VERSION      = "2.1.3"
RIAK_DOWNLOAD_URL = "https://s3.amazonaws.com/downloads.basho.com/riak/2.1/#{RIAK_VERSION}/osx/10.8/riak-#{RIAK_VERSION}-OSX-x86_64.tar.gz"
NUM_NODES = 5
RING_SIZE = 16
BACKEND = 'leveldb' #options: bitcask, leveldb, memory.

task :default => :help

task :help do
  sh %{rake -T}
end

desc "install, start, and join riak nodes"
task :bootstrap => [:install, :start, :join]

desc "start all riak nodes"
task :start do
  (1..NUM_NODES).each do |n|
    sh %{ulimit -n 65536; ./riak#{n}/bin/riak start}
  end
  puts "========================================"
  puts "Riak Dev Cluster started"
  puts
  puts "HTTP API: http://127.0.0.1:11098"
  puts
  puts "Admin UI: http://127.0.0.1:11098/admin"
  puts "========================================"
end

desc "stop all riak nodes"
task :stop do
  (1..NUM_NODES).each do |n|
    sh %{ulimit -n 65536; ./riak#{n}/bin/riak stop} rescue "not running"
  end
end

desc "restart all riak nodes"
task :restart => [:stop, :start]

desc "join riak nodes (only needed once)"
  task :join do
    (2..NUM_NODES).each do |n|
      sh %{./riak#{n}/bin/riak-admin join -f riak1@127.0.0.1} rescue "already joined"
  end
end

desc "clear data from all riak nodes"
  task :clear => :stop do
    (1..NUM_NODES).each do |n|
      sh %{rm -rf riak#{n}}
  end
end

desc "install riak"
  task :install => [:fetch_riak, :copy_riak]

desc "ping all riak nodes"
  task :ping do
    (1..NUM_NODES).each do |n|
      sh %{riak#{n}/bin/riak ping}
  end
end

desc "riak-admin member-status"
  task :member_status do
     sh %{riak2/bin/riak-admin member-status}
       end 
       
desc "riak-admin test"
           task :test do
            sh %{riak1/bin/riak-admin test}
            sh %{riak2/bin/riak-admin test}
            sh %{riak3/bin/riak-admin test}
            sh %{riak4/bin/riak-admin test}
            sh %{riak5/bin/riak-admin test}
            end 
  
desc "riak-admin status"
  task :status do
    sh %{riak1/bin/riak-admin status}
          end   
          

desc "riak-admin ring-status"
  task :ring_status do
      sh %{riak1/bin/riak-admin ring-status}
          end   

          
 desc "set up maps bucket-type"
   task :map_bucket do  
     sh %{riak1/bin/riak-admin bucket-type create maps '{"props":{"datatype":"map"}}'}
     sh %{riak1/bin/riak-admin bucket-type activate maps}
          end

 desc "set up sets bucket-type"
   task :set_bucket do  
     sh %{riak1/bin/riak-admin bucket-type create sets '{"props":{"datatype":"set"}}'}
     sh %{riak1/bin/riak-admin bucket-type activate sets}
          end

 desc "set up counters bucket-type"
   task :counter_bucket do  
     sh %{riak1/bin/riak-admin bucket-type create counters '{"props":{"datatype":"counter"}}'}
     sh %{riak1/bin/riak-admin bucket-type activate counters}
          end

 task :fetch_riak do
   sh "curl -OL #{RIAK_DOWNLOAD_URL}" unless File.exist? "riak-#{RIAK_VERSION}-OSX-x86_64.tar.gz"
   sh "shasum -a 256 -c SHA256SUM && tar xzf riak-#{RIAK_VERSION}-OSX-x86_64.tar.gz" unless File.exist? "riak-#{RIAK_VERSION}"
 end

 task :copy_riak do
   (1..NUM_NODES).each do |n|
     system %{cp -nr riak-#{RIAK_VERSION}/ riak#{n}}
    system %(sed -i '' 's/riak@127.0.0.1/riak#{n}@127.0.0.1/' riak#{n}/etc/riak.conf)
    system %(sed -i '' 's/127.0.0.1:8098/127.0.0.1:1#{n}098/' riak#{n}/etc/riak.conf)
    system %(sed -i '' 's/127.0.0.1:8087/127.0.0.1:1#{n}087/' riak#{n}/etc/riak.conf)
    system %(echo 'riak_control = on' >> riak#{n}/etc/riak.conf)
    system %(echo 'handoff.port = 1#{n}099' >> riak#{n}/etc/riak.conf)
    system %(echo 'ring_size = #{RING_SIZE}' >> riak#{n}/etc/riak.conf)
    system %(echo 'storage_backend = #{BACKEND}' >> riak#{n}/etc/riak.conf)
   end
 end
