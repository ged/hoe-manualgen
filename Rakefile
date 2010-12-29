#!/usr/bin/env rake

require 'hoe'

# 1.9.2 and later require explicit relative require
if defined?( require_relative )
	$stderr.puts "Requiring relative lib/hoe/manualgen..."
	require_relative "lib/hoe/manualgen"
	$stderr.puts "	require done."
else
	$LOAD_PATH.unshift( 'lib' )
	require 'hoe/manualgen'
end

Hoe.plugin :mercurial
Hoe.plugin :yard
Hoe.plugin :signing
Hoe.plugin :manualgen

Hoe.plugins.delete :rubyforge

hoespec = Hoe.spec 'hoe-manualgen' do
	self.readme_file = 'README.md'

	self.developer 'Michael Granger', 'ged@FaerieMUD.org'

	self.extra_dev_deps <<
		['rspec', '~> 2.1.0']

	self.spec_extras[:licenses] = ["BSD"]
	self.spec_extras[:signing_key] = '/Volumes/Keys/ged-private_gem_key.pem'

	self.require_ruby_version( '>=1.8.7' )

	self.hg_sign_tags = true if self.respond_to?( :hg_sign_tags= )
	self.yard_opts = [ '--use-cache', '--protected', '--verbose' ] if
		self.respond_to?( :yard_opts )

	self.rdoc_locations << "deveiate:/usr/local/www/public/code/#{remote_rdoc_dir}"
end

ENV['VERSION'] ||= hoespec.spec.version.to_s

begin
	include Hoe::MercurialHelpers

	### Task: prerelease
	desc "Append the package build number to package versions"
	task :pre do
		rev = get_numeric_rev()
		trace "Current rev is: %p" % [ rev ]
		hoespec.spec.version.version << "pre#{rev}"
		Rake::Task[:gem].clear

		Gem::PackageTask.new( hoespec.spec ) do |pkg|
			pkg.need_zip = true
			pkg.need_tar = true
		end
	end

	### Make the ChangeLog update if the repo has changed since it was last built
	file '.hg/branch'
	file 'ChangeLog' => '.hg/branch' do |task|
		$stderr.puts "Updating the changelog..."
		content = make_changelog()
		File.open( task.name, 'w', 0644 ) do |fh|
			fh.print( content )
		end
	end

	# Rebuild the ChangeLog immediately before release
	task :prerelease => 'ChangeLog'

rescue NameError => err
	task :no_hg_helpers do
		fail "Couldn't define the :pre task: %s: %s" % [ err.class.name, err.message ]
	end

	task :pre => :no_hg_helpers
	task 'ChangeLog' => :no_hg_helpers

end

