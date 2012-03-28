#!/usr/bin/env rake

require 'pathname'

begin
	require 'hoe'
rescue LoadError
	abort "This Rakefile requires Hoe (gem install hoe)"
end

require 'rake/clean'

Hoe.add_include_dirs 'lib', 'data/hoe-manualgen'

Hoe.plugin :mercurial
Hoe.plugin :signing
Hoe.plugin :manualgen

Hoe.plugins.delete :rubyforge

hoespec = Hoe.spec 'hoe-manualgen' do
	self.readme_file = 'README.rdoc'
	self.history_file = 'History.rdoc'
	self.extra_rdoc_files = FileList[ '*.rdoc', 'data/hoe-manualgen/lib/*.rb' ]

	self.developer 'Michael Granger', 'ged@FaerieMUD.org'

	self.dependency 'hoe', "~> #{Hoe::VERSION[/^\d+\.\d+/]}"
	self.dependency 'RedCloth', '~> 4.2'
	self.dependency 'rcodetools', '~> 0.8'

	self.dependency 'tidy-ext', '~> 0.1', :developer

	self.manual_lib_dir = Pathname.pwd + 'data/hoe-manualgen/lib'

	self.spec_extras[:licenses] = ["BSD"]
	self.require_ruby_version( '>=1.8.7' )

	self.hg_sign_tags = true if self.respond_to?( :hg_sign_tags= )
	self.rdoc_locations << "deveiate:/usr/local/www/public/code/#{remote_rdoc_dir}"
end

ENV['VERSION'] ||= hoespec.spec.version.to_s

