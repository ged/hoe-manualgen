#!/usr/bin/env rake

begin
	require 'hoe'
rescue LoadError
	$stderr.puts "This Rakefile requires Hoe (gem install hoe)"
end

require 'rake/clean'

Hoe.add_include_dirs 'lib'

Hoe.plugin :mercurial
Hoe.plugin :signing
Hoe.plugin :manualgen

Hoe.plugins.delete :rubyforge

hoespec = Hoe.spec 'hoe-manualgen' do
	self.readme_file = 'README.md'
	self.history_file = 'History.md'

	self.developer 'Michael Granger', 'ged@FaerieMUD.org'

	self.extra_deps.push *{
		'hoe'        => "~> #{Hoe::VERSION[/^\d+\.\d+/]}",
		'RedCloth'   => '~> 4.2',
		'rcodetools' => '~> 0.8',
	}
	self.extra_dev_deps.push *{
		'rspec'    => '~> 2.4',
		'tidy-ext' => '~> 0.1',
	}

	self.spec_extras[:licenses] = ["BSD"]
	self.spec_extras[:signing_key] = '/Volumes/Keys/ged-private_gem_key.pem'

	self.require_ruby_version( '>=1.8.7' )

	self.hg_sign_tags = true if self.respond_to?( :hg_sign_tags= )

	self.extra_rdoc_files += Dir.glob( 'data/hoe-manualgen/lib/*.rb' )
	self.rdoc_locations << "deveiate:/usr/local/www/public/code/#{remote_rdoc_dir}"
end

ENV['VERSION'] ||= hoespec.spec.version.to_s

