Code: https://github.com/jordansissel/
Blog: http://www.semicomplete.com/
SysAdvent: http://sysadvent.blogspot.com/

"I need to deploy this 
           application we wrote"
You wrote an app

Now you need to deploy it.
Version it?

roll your own package
solution:
... but how?
so maybe you should roll your own package

tar?
rsync?
svn?
git?
rpm?
deb?
bittorrent?
shar?
jar?
What tools can you use? Should you use?

tar? git? jar? rsync? rpm?

Some of these aren't straight "package" tools, but they can be combined to make a system that does what you want.

Native packages are a good
Tracks important things.
native packages implement much of what you want

Why native packages?
Dependency resolution

Versioned

File checksums

Upgrades are supported

Already easy to distribute (apt-get, yum, etc)

Queryable (dpkg -l, rpm -qa, yum search, etc)
Why?

dependency resolution

versioned

file checksums

upgrades

already easy to distribute

queryable


Native packages are a good
Lots of tool support.
native packages are good because there's lots of tool support.

Usage already understood
RHEL/CentOS/etc
rpm, yum, createrepo, mrepo


Debian/Ubuntu:
dpkg, apt-{get,cache}, apt-ftparchive, reprepro

the rpm/yum tools are known tols

dpkg and apt-get tools are known.

the repo tools are fairly well known.

Easy to get support or find help for these things.

puppet:
  package {
    "apache": ensure => "2.2.ourpatches";
  }

chef:
  package "apache" do
    version "2.2.ourpatches"
    action :install
  end
puppet and chef support nagive packages by default.

but...
but...

CentOS/RHEL/Fedora - RPM
There is a book on using and writing RPMs.
 
         An effing book.


                      You don't have time for this crap.



  In case you have time for this crap,
                                                http://www.rpm.org/max-rpm/
RPM has a bookt...

it's called maximum rpm

it's a book.

I don't have the energy to read a book.

In case you have time ,url there.

Debian/Ubuntu - DEB
Debian package docs are not for you.

               You are probably not a debian maintainer.

  You don't care about debian policy, do you? 

                               Me either.


   In case you care about the debian maintainer guide,
                         http://www.debian.org/doc/maint-guide/
Debians package docs are mixed in the maintainer and policy guide.

I am not maintaining packages for debian. I'm building packages for my company.

Debian's policy is not my policy.

for reference
:)
This is how I want to feel.

smiley

writing rpm specs, building them, etc.
:(
how I feel when writing rpm specs, building them, etc.

writing debian control files, fighting the system
:(
how I feel when writing debian control files

I always end up fighting debian's packaging tools because they embed debian policy in the tools.

trying to build a deb package with debug symbols
>:(
get angry trying to build a deb with debug symbols only to have it stripped automatically, without asking me, during the package build step

switching from in-house rpms to debs.
:'(
sadness when switching systems

my previous jobs used centos

now I use ubuntu in production.

rpm and deb tools are totally unrelated.

dealing with broken postinst/prerm scripts
maintainer scripts are almost always poorly written shell scripts that break easily.

I want to smash things when broken postinstall or prerm scripts disable the entire apt-get system.

Example: mysql restarting on package upgrades. collectd's prerm breaking during uninstall, etc.

unicode frowny face
☹
This is a unicode frownyface.

Keep your sanity.
use fpm.
Keep your sanity.

Use fpm

fpm is unopinionated software
By unopinionated I mean I try to keep fpm useful as a tool, not as a policy enforcer or personal philosophy implementation.

If there's a packaging task fpm cannot do today, let me know, and I will try to make it work.

gem install fpm
(you can later package fpm with fpm, if you want)
How to install it?

gem install fpm


and yes, later you can use fpm to package itself for distribution later.

you might be thinking "NOO NOT RUBY, GET OFF MY LAWN"

I am committed to supporting whatever version of ruby folks are using.

fpm -s <source_type> -t <target_type> ...
Basic usage

-s is source
-t is target

rpm
npm
rubygem
deb
python
dir(ectory)
rpm

deb

solaris

puppet
Output
Input
A pacman flow chart of inputs and outputs

Convert files/dirs to rpm.
% ./configure
% make all install DESTDIR=buildoutput


% fpm -s dir -t rpm \
    -v 1.0 -n mypackage 
    -C buildoutput .

Simple example of building a package from a normal project that uses configure and make.

Convert files/dirs to deb
% wget http://nodejs.org/dist/node-v0.4.6.tar.gz
% tar -zxf node-v0.4.6.tar.gz
% cd node-v0.4.6
% ./configure --prefix=/usr
% make
% mkdir /tmp/installdir
% make install DESTDIR=/tmp/installdir
% fpm -s dir -t deb -n nodejs -v 0.4.6 \
    -C /tmp/installdir \
    -d "libssl0.9.8 (>= 0)" \
    -d "libstdc++6 (>= 4.4.3)" \
    usr/bin usr/lib

Created .../nodejs-0.4.6_amd64.deb



Build a nodejs .deb package trivially.


Convert files/dirs to deb
% dpkg -c nodejs-0.4.6_amd64.deb | grep bin/[a-z]
-rwxr-xr-x root/root   8967683 2011-04-15 01:34 usr/bin/node
-rwxr-xr-x root/root       355 2011-04-13 21:20 usr/bin/node-waf

% sudo dpkg -i nodejs-0.4.6_amd64.deb
...
Setting up nodejs (0.4.6) ...

% /usr/bin/node
> console.log("Hello");
Hello

inspect our new package

install it

and now run it.

Convert rubygem to deb.
% fpm -s gem -t deb -v 3.0.5 rails

Trying to download rails (version=3.0.5)
...
Created ... rubygem-rails_3.0.5_amd64.deb
bulit in support for rubygems

give the gem name, the version (optional, latest otherwise)

Done.

Convert rubygem to deb.
% dpkg --info rubygem-rails_3.0.5_amd64.deb 
 Package: rubygem-rails
 Version: 3.0.5
 Maintainer: David Heinemeier Hansson
 Depends: rubygem-activesupport (= 3.0.5), ...






               
(output trimmed for content)
inspect our new package.

maintainer picked automatically

dependencies picked automatically


Convert rubygem to rpm.
% fpm -s gem -t rpm -v 3.0.5 rails

Trying to download rails (version=3.0.5)
...
Created .../rubygem-rails-3.0.5.x86_64.rpm

Same basic thing to convert a gem to rpm.

Convert rubygem to rpm.
% rpm -qp rubygem-rails-3.0.5.x86_64.rpm --info
Name        : rubygem-rails
Version     : 3.0.5
Release     : 1
Summary     : Full-stack web application framework.

% rpm -qp rubygem-rails-3.0.5.x86_64.rpm -R
rubygem-activesupport = 3.0.5
rubygem-actionpack = 3.0.5
rubygem-activerecord = 3.0.5
...
              
(output trimmed for content)
inspect our new rpm

That package is wrong!
rubygem-rails-3.0.5.x86_64.rpm
WRONG ARCH. DUDE.
If only we could fix it...
You might have noticed

Rails is pure ruby, so the x86_64 arch is not really correct.

Convert an rpm to an rpm.
% fpm -s rpm -t rpm -a noarch \
  rubygem-rails-3.0.5.x86_64.rpm

Created /.../rubygem-rails-3.0.5-1.noarch.rpm

% rpm -qp rubygem-rails-3.0.5-1.noarch.rpm \
  --qf "%{arch}\n"

noarch



Input an rpm, output an rpm, change the architecture in the package.

Edit an existing RPM (--edit)
% fpm -s rpm -t rpm --edit rubygem-rails-3.0.5-1.noarch.rpm  

Opens rpm specfile in $EDITOR

Save, quit, finishes building. 

                                                                       Win.
If you want to edit the rpm spec or debian control file before packaging, use the edit flag.

What do I do?
Build internal apps with version "rev.branch"

svn: revisions are useful numbers

git: use commit timestamps instead of SHA1 revs

package {
  "loggly-frontend": 
    ensure => "123456.master";
}

Easy to switch branches/versions now.
So what do I do?

build internal apps using versioning <revision>.<branch>

svn revisions are good

git does'nt have revision numbers, so we use the last-commit timestamp

example of puppet installing 



Caveat - name conflicts
Problem: Can't install two packages by the same name.

Solution: Include the version in the "name"

% fpm ... -n myname-${version} -v ${version} ...
% dpkg --info rvm-ruby-1.9.2p180_1.9.2p180-1_amd64.deb
 Package: rvm-ruby-1.9.2p180
 Version: 1.9.2p180-1

Some deployment tools let you install multiple versoins of something and a symlink is kept pointing at the current or active version.

You can't do this without workarounds.

rpm and deb both will reject or replace packages by the same name during install - so you have to include the version number in the name to make it a new "package"

Also you'll need to make sure that files installed don't conflict, but that should be obvious.

Caveat - single-version disease
Problem: Barf on multiple versions of a package.

Solution: Hack around it if you can.

Example: reprepro (debian) deletes old versions unless told otherwise. It also will only generate a package manifest with 1 version of each package

Work around: use apt-ftparchive
   Make sure you use the '--db foo.db' flag for speed.
This is my pet peeve.

Debian and RHEL assume only one version of anything, ever.

You can feel the groans and pains of the system when you install things that have multiple versions (like ruby or python, everything is "ruby18-foo" or python27-bar)

Upgrade and it broke? There is no rolling back when only one version is available.

I'll show you what we do at loggly shortly.

Caveat - yum isn't yummy.
Problem: Yum misses lots of key features by default

Solution: plugins (downgrade plugin, etc)

Better solution*:  apt-rpm.

apt-rpm is apt-get using RPMs.


URL: http://apt-rpm.org/
            * in my opinion, apt-rpm is better than yum.
yum can't downgrade by default, among other things.

I like apt-rpm better since it supports more features I care about.

Multiple package versions available?
% apt-cache policy loggly-frontend
loggly-frontend:
  Installed: (none)
  Candidate: 20110418232451.trunk
  Version table:
     20110418232451.trunk 0
     20110418210530.trunk 0
     4094.trunk 0
     4093.4023hotfix 0
     4089.4023hotfix 0
     4087.4023hotfix 0
     4086.4023hotfix 0
     4085.trunk 0
... (output trimmed for content) ...
Available versions for install.
Example:
apt-get install \
 loggly-frontend=4094.trunk
The 'apt-cache policy' tool will let you ask what will be installed by default, what is available, etc.

Related Projects
https://github.com/jordansissel/node-packaging

https://github.com/jordansissel/gem-packaging

https://github.com/jordansissel/rvm-packaging

https://github.com/jordansissel/python-packaging
Some related projects

packaging nodejs packages, gems, rvm (ruby version manager), and python modules

Live Demo Request Party                     
What can I package for you right now?
I have a python 3.2 example prepared to demo.

I am open to other software (assuming it downloads/compiles quickly)

rubygems?
python modules?
tools?
other software?
something else?

Questions?
Twitter: @jordansissel

Project: http://github.com/jordansissel/fpm

Problems: Email, github issue, twitter, don't care how you contact me.

Wiki (lots of examples):
    https://github.com/jordansissel/fpm/wiki

I'm 'whack' on irc (efnet, freenode)
questions?

here is how to contac tme. 

If you use fpm, I want feedback. If it misses a feature, tell me or send me patches.

The wiki has lots of good examples for using fpm in various scenarios

References
Troll face
http://en.wikipedia.org/wiki/Troll_(Internet)

Laser dance party photo:   
http://www.flickr.com/photos/maartendive/4764534725/in/photostream/

Rage face:
http://s284.photobucket.com/albums/ll37/Geesaroni/?action=view&current=RageFace.png&newest=1

Loggly beaver+axe background
http://www.loggly.com/swag/
Intro:

  This is fpm version 1.6.1

  If you think something is wrong, it's probably a bug! :)
  Please file these here: https://github.com/jordansissel/fpm/issues

  You can find support on irc (#fpm on freenode irc) or via email with
  fpm-users@googlegroups.com

Loaded package types:
  - dir
  - gem
  - deb
  - npm
  - rpm
  - tar
  - cpan
  - pear
  - empty
  - puppet
  - python
  - osxpkg
  - solaris
  - p5p
  - pkgin
  - freebsd
  - zip
  - virtualenv
  - sh
  - pleaserun
  - apk
  - pacman

Usage:
    fpm [OPTIONS] [ARGS] ...

Parameters:
    [ARGS] ...                    Inputs to the source package type. For the 'dir' type, this is the files and directories you want to include in the package. For others, like 'gem', it specifies the packages to download and use as the gem input


Options:
    -t OUTPUT_TYPE                the type of package you want to create (deb, rpm, solaris, etc)
    -s INPUT_TYPE                 the package type to use as input (gem, rpm, python, etc)
    -C CHDIR                      Change directory to here before searching for files
    --prefix PREFIX               A path to prefix files with when building the target package. This may not be necessary for all input packages. For example, the 'gem' type will prefix with your gem directory automatically.
    -p, --package OUTPUT          The package file path to output.
    -f, --force                   Force output even if it will overwrite an existing file (default: false)
    -n, --name NAME               The name to give to the package
    --log LEVEL                   Set the log level. Values: error, warn, info, debug.
    --verbose                     Enable verbose output
    --debug                       Enable debug output
    --debug-workspace             Keep any file workspaces around for debugging. This will disable automatic cleanup of package staging and build paths. It will also print which directories are available.
    -v, --version VERSION         The version to give to the package (default: 1.0)
    --iteration ITERATION         The iteration to give to the package. RPM calls this the 'release'. FreeBSD calls it 'PORTREVISION'. Debian calls this 'debian_revision'
    --epoch EPOCH                 The epoch value for this package. RPM and Debian calls this 'epoch'. FreeBSD calls this 'PORTEPOCH'
    --license LICENSE             (optional) license name for this package
    --vendor VENDOR               (optional) vendor name for this package
    --category CATEGORY           (optional) category this package belongs to (default: "none")
    -d, --depends DEPENDENCY      A dependency. This flag can be specified multiple times. Value is usually in the form of: -d 'name' or -d 'name > version'
    --no-depends                  Do not list any dependencies in this package (default: false)
    --no-auto-depends             Do not list any dependencies in this package automatically (default: false)
    --provides PROVIDES           What this package provides (usually a name). This flag can be specified multiple times.
    --conflicts CONFLICTS         Other packages/versions this package conflicts with. This flag can be specified multiple times.
    --replaces REPLACES           Other packages/versions this package replaces. Equivalent of rpm's 'Obsoletes'. This flag can be specified multiple times.
    --config-files CONFIG_FILES   Mark a file in the package as being a config file. This uses 'conffiles' in debs and %config in rpm. If you have multiple files to mark as configuration files, specify this flag multiple times.  If argument is directory all files inside it will be recursively marked as config files.
    --directories DIRECTORIES     Recursively mark a directory as being owned by the package. Use this flag multiple times if you have multiple directories and they are not under the same parent directory 
    -a, --architecture ARCHITECTURE The architecture name. Usually matches 'uname -m'. For automatic values, you can use '-a all' or '-a native'. These two strings will be translated into the correct value for your platform and target package type.
    -m, --maintainer MAINTAINER   The maintainer of this package. (default: "<orkung@orkung-onair>")
    -S, --package-name-suffix PACKAGE_NAME_SUFFIX a name suffix to append to package and dependencies.
    -e, --edit                    Edit the package spec before building. (default: false)
    -x, --exclude EXCLUDE_PATTERN Exclude paths matching pattern (shell wildcard globs valid here). If you have multiple file patterns to exclude, specify this flag multiple times.
    --exclude-file EXCLUDE_PATH   The path to a file containing a newline-sparated list of patterns to exclude from input.
    --description DESCRIPTION     Add a description for this package. You can include '\n' sequences to indicate newline breaks. (default: "no description")
    --url URI                     Add a url for this package. (default: "http://example.com/no-uri-given")
    --inputs INPUTS_PATH          The path to a file containing a newline-separated list of files and dirs to use as input.
    --post-install FILE           (DEPRECATED, use --after-install) A script to be run after package installation
    --pre-install FILE            (DEPRECATED, use --before-install) A script to be run before package installation
    --post-uninstall FILE         (DEPRECATED, use --after-remove) A script to be run after package removal
    --pre-uninstall FILE          (DEPRECATED, use --before-remove) A script to be run before package removal
    --after-install FILE          A script to be run after package installation
    --before-install FILE         A script to be run before package installation
    --after-remove FILE           A script to be run after package removal
    --before-remove FILE          A script to be run before package removal
    --after-upgrade FILE          A script to be run after package upgrade. If not specified,
                                  --before-install, --after-install, --before-remove, and 
                                  --after-remove will behave in a backwards-compatible manner
                                  (they will not be upgrade-case aware).
                                  Currently only supports deb, rpm and pacman packages.
    --before-upgrade FILE         A script to be run before package upgrade. If not specified,
                                  --before-install, --after-install, --before-remove, and 
                                  --after-remove will behave in a backwards-compatible manner
                                  (they will not be upgrade-case aware).
                                  Currently only supports deb, rpm and pacman packages.
    --template-scripts            Allow scripts to be templated. This lets you use ERB to template your packaging scripts (for --after-install, etc). For example, you can do things like <%= name %> to get the package name. For more information, see the fpm wiki: https://github.com/jordansissel/fpm/wiki/Script-Templates
    --template-value KEY=VALUE    Make 'key' available in script templates, so <%= key %> given will be the provided value. Implies --template-scripts
    --workdir WORKDIR             The directory you want fpm to do its work in, where 'work' is any file copying, downloading, etc. Roughly any scratch space fpm needs to build your package. (default: "/tmp")
    --gem-bin-path DIRECTORY      (gem only) The directory to install gem executables
    --gem-package-prefix NAMEPREFIX (gem only) (DEPRECATED, use --package-name-prefix) Name to prefix the package name with.
    --gem-package-name-prefix PREFIX (gem only) Name to prefix the package name with. (default: "rubygem")
    --gem-gem PATH_TO_GEM         (gem only) The path to the 'gem' tool (defaults to 'gem' and searches your $PATH) (default: "gem")
    --[no-]gem-fix-name           (gem only) Should the target package name be prefixed? (default: true)
    --[no-]gem-fix-dependencies   (gem only) Should the package dependencies be prefixed? (default: true)
    --[no-]gem-env-shebang        (gem only) Should the target package have the shebang rewritten to use env? (default: true)
    --[no-]gem-prerelease         (gem only) Allow prerelease versions of a gem (default: false)
    --gem-disable-dependency gem_name (gem only) The gem name to remove from dependency list
    --[no-]gem-version-bins       (gem only) Append the version to the bins (default: false)
    --[no-]deb-ignore-iteration-in-dependencies (deb only) For '=' (equal) dependencies, allow iterations on the specified version. Default is to be specific. This option allows the same version of a package but any iteration is permitted
    --deb-build-depends DEPENDENCY (deb only) Add DEPENDENCY as a Build-Depends
    --deb-pre-depends DEPENDENCY  (deb only) Add DEPENDENCY as a Pre-Depends
    --deb-compression COMPRESSION (deb only) The compression type to use, must be one of gz, bzip2, xz. (default: "gz")
    --deb-custom-control FILEPATH (deb only) Custom version of the Debian control file.
    --deb-config SCRIPTPATH       (deb only) Add SCRIPTPATH as debconf config file.
    --deb-templates FILEPATH      (deb only) Add FILEPATH as debconf templates file.
    --deb-installed-size KILOBYTES (deb only) The installed size, in kilobytes. If omitted, this will be calculated automatically
    --deb-priority PRIORITY       (deb only) The debian package 'priority' value. (default: "extra")
    --[no-]deb-use-file-permissions (deb only) Use existing file permissions when defining ownership and modes
    --deb-user USER               (deb only) The owner of files in this package (default: "root")
    --deb-group GROUP             (deb only) The group owner of files in this package (default: "root")
    --deb-changelog FILEPATH      (deb only) Add FILEPATH as debian changelog
    --deb-upstream-changelog FILEPATH (deb only) Add FILEPATH as upstream changelog
    --deb-recommends PACKAGE      (deb only) Add PACKAGE to Recommends
    --deb-suggests PACKAGE        (deb only) Add PACKAGE to Suggests
    --deb-meta-file FILEPATH      (deb only) Add FILEPATH to DEBIAN directory
    --deb-interest EVENT          (deb only) Package is interested in EVENT trigger
    --deb-activate EVENT          (deb only) Package activates EVENT trigger
    --deb-field 'FIELD: VALUE'    (deb only) Add custom field to the control file
    --[no-]deb-no-default-config-files (deb only) Do not add all files in /etc as configuration files by default for Debian packages. (default: false)
    --[no-]deb-auto-config-files  (deb only) Init script and default configuration files will be labeled as configuration files for Debian packages. (default: true)
    --deb-shlibs SHLIBS           (deb only) Include control/shlibs content. This flag expects a string that is used as the contents of the shlibs file. See the following url for a description of this file and its format: http://www.debian.org/doc/debian-policy/ch-sharedlibs.html#s-shlibs
    --deb-init FILEPATH           (deb only) Add FILEPATH as an init script
    --deb-default FILEPATH        (deb only) Add FILEPATH as /etc/default configuration
    --deb-upstart FILEPATH        (deb only) Add FILEPATH as an upstart script
    --deb-systemd FILEPATH        (deb only) Add FILEPATH as a systemd script
    --[no-]deb-systemd-restart-after-upgrade (deb only) Restart service after upgrade (default: true)
    --npm-bin NPM_EXECUTABLE      (npm only) The path to the npm executable you wish to run. (default: "npm")
    --npm-package-name-prefix PREFIX (npm only) Name to prefix the package name with. (default: "node")
    --npm-registry NPM_REGISTRY   (npm only) The npm registry to use instead of the default.
    --[no-]rpm-use-file-permissions (rpm only) Use existing file permissions when defining ownership and modes.
    --rpm-user USER               (rpm only) Set the user to USER in the %files section. Overrides the user when used with use-file-permissions setting.
    --rpm-group GROUP             (rpm only) Set the group to GROUP in the %files section. Overrides the group when used with use-file-permissions setting.
    --rpm-defattrfile ATTR        (rpm only) Set the default file mode (%defattr). (default: "-")
    --rpm-defattrdir ATTR         (rpm only) Set the default dir mode (%defattr). (default: "-")
    --rpm-rpmbuild-define DEFINITION (rpm only) Pass a --define argument to rpmbuild.
    --rpm-dist DIST-TAG           (rpm only) Set the rpm distribution.
    --rpm-digest md5|sha1|sha256|sha384|sha512 (rpm only) Select a digest algorithm. md5 works on the most platforms. (default: "md5")
    --rpm-compression none|xz|gzip|bzip2 (rpm only) Select a compression method. gzip works on the most platforms. (default: "gzip")
    --rpm-os OS                   (rpm only) The operating system to target this rpm for. You want to set this to 'linux' if you are using fpm on OS X, for example
    --rpm-changelog FILEPATH      (rpm only) Add changelog from FILEPATH contents
    --rpm-summary SUMMARY         (rpm only) Set the RPM summary. Overrides the first line on the description if set
    --[no-]rpm-sign               (rpm only) Pass --sign to rpmbuild
    --[no-]rpm-auto-add-directories (rpm only) Auto add directories not part of filesystem
    --rpm-auto-add-exclude-directories DIRECTORIES (rpm only) Additional directories ignored by '--rpm-auto-add-directories' flag
    --[no-]rpm-autoreqprov        (rpm only) Enable RPM's AutoReqProv option
    --[no-]rpm-autoreq            (rpm only) Enable RPM's AutoReq option
    --[no-]rpm-autoprov           (rpm only) Enable RPM's AutoProv option
    --rpm-attr ATTRFILE           (rpm only) Set the attribute for a file (%attr), e.g. --rpm-attr 750,user1,group1:/some/file
    --rpm-init FILEPATH           (rpm only) Add FILEPATH as an init script
    --rpm-filter-from-provides REGEX (rpm only) Set %filter_from_provides to the supplied REGEX.
    --rpm-filter-from-requires REGEX (rpm only) Set %filter_from_requires to the supplied REGEX.
    --rpm-tag TAG                 (rpm only) Adds a custom tag in the spec file as is. Example: --rpm-tag 'Requires(post): /usr/sbin/alternatives'
    --[no-]rpm-ignore-iteration-in-dependencies (rpm only) For '=' (equal) dependencies, allow iterations on the specified version. Default is to be specific. This option allows the same version of a package but any iteration is permitted
    --[no-]rpm-verbatim-gem-dependencies (rpm only) When converting from a gem, leave the old (fpm 0.4.x) style dependency names. This flag will use the old 'rubygem-foo' names in rpm requires instead of the redhat style rubygem(foo). (default: false)
    --rpm-verifyscript FILE       (rpm only) a script to be run on verification
    --rpm-pretrans FILE           (rpm only) pretrans script
    --rpm-posttrans FILE          (rpm only) posttrans script
    --rpm-trigger-before-install '[OPT]PACKAGE: FILEPATH' (rpm only) Adds a rpm trigger script located in FILEPATH, having 'OPT' options and linking to 'PACKAGE'. PACKAGE can be a comma seperated list of packages. See: http://rpm.org/api/4.4.2.2/triggers.html
    --rpm-trigger-after-install '[OPT]PACKAGE: FILEPATH' (rpm only) Adds a rpm trigger script located in FILEPATH, having 'OPT' options and linking to 'PACKAGE'. PACKAGE can be a comma seperated list of packages. See: http://rpm.org/api/4.4.2.2/triggers.html
    --rpm-trigger-before-uninstall '[OPT]PACKAGE: FILEPATH' (rpm only) Adds a rpm trigger script located in FILEPATH, having 'OPT' options and linking to 'PACKAGE'. PACKAGE can be a comma seperated list of packages. See: http://rpm.org/api/4.4.2.2/triggers.html
    --rpm-trigger-after-target-uninstall '[OPT]PACKAGE: FILEPATH' (rpm only) Adds a rpm trigger script located in FILEPATH, having 'OPT' options and linking to 'PACKAGE'. PACKAGE can be a comma seperated list of packages. See: http://rpm.org/api/4.4.2.2/triggers.html
    --cpan-perl-bin PERL_EXECUTABLE (cpan only) The path to the perl executable you wish to run. (default: "perl")
    --cpan-cpanm-bin CPANM_EXECUTABLE (cpan only) The path to the cpanm executable you wish to run. (default: "cpanm")
    --cpan-mirror CPAN_MIRROR     (cpan only) The CPAN mirror to use instead of the default.
    --[no-]cpan-mirror-only       (cpan only) Only use the specified mirror for metadata. (default: false)
    --cpan-package-name-prefix NAME_PREFIX (cpan only) Name to prefix the package name with. (default: "perl")
    --[no-]cpan-test              (cpan only) Run the tests before packaging? (default: true)
    --cpan-perl-lib-path PERL_LIB_PATH (cpan only) Path of target Perl Libraries
    --[no-]cpan-sandbox-non-core  (cpan only) Sandbox all non-core modules, even if they're already installed (default: true)
    --[no-]cpan-cpanm-force       (cpan only) Pass the --force parameter to cpanm (default: false)
    --pear-package-name-prefix PREFIX (pear only) Name prefix for pear package (default: "php-pear")
    --pear-channel CHANNEL_URL    (pear only) The pear channel url to use instead of the default.
    --[no-]pear-channel-update    (pear only) call 'pear channel-update' prior to installation
    --pear-bin-dir BIN_DIR        (pear only) Directory to put binaries in
    --pear-php-bin PHP_BIN        (pear only) Specify php executable path if differs from the os used for packaging
    --pear-php-dir PHP_DIR        (pear only) Specify php dir relative to prefix if differs from pear default (pear/php)
    --pear-data-dir DATA_DIR      (pear only) Specify php dir relative to prefix if differs from pear default (pear/data)
    --python-bin PYTHON_EXECUTABLE (python only) The path to the python executable you wish to run. (default: "python")
    --python-easyinstall EASYINSTALL_EXECUTABLE (python only) The path to the easy_install executable tool (default: "easy_install")
    --python-pip PIP_EXECUTABLE   (python only) The path to the pip executable tool. If not specified, easy_install is used instead (default: nil)
    --python-pypi PYPI_URL        (python only) PyPi Server uri for retrieving packages. (default: "https://pypi.python.org/simple")
    --python-package-prefix NAMEPREFIX (python only) (DEPRECATED, use --package-name-prefix) Name to prefix the package name with.
    --python-package-name-prefix PREFIX (python only) Name to prefix the package name with. (default: "python")
    --[no-]python-fix-name        (python only) Should the target package name be prefixed? (default: true)
    --[no-]python-fix-dependencies (python only) Should the package dependencies be prefixed? (default: true)
    --[no-]python-downcase-name   (python only) Should the target package name be in lowercase? (default: true)
    --[no-]python-downcase-dependencies (python only) Should the package dependencies be in lowercase? (default: true)
    --python-install-bin BIN_PATH (python only) The path to where python scripts should be installed to.
    --python-install-lib LIB_PATH (python only) The path to where python libs should be installed to (default depends on your python installation). Want to find out what your target platform is using? Run this: python -c 'from distutils.sysconfig import get_python_lib; print get_python_lib()'
    --python-install-data DATA_PATH (python only) The path to where data should be installed to. This is equivalent to 'python setup.py --install-data DATA_PATH
    --[no-]python-dependencies    (python only) Include requirements defined in setup.py as dependencies. (default: true)
    --[no-]python-obey-requirements-txt (python only) Use a requirements.txt file in the top-level directory of the python package for dependency detection. (default: false)
    --python-scripts-executable PYTHON_EXECUTABLE (python only) Set custom python interpreter in installing scripts. By default distutils will replace python interpreter in installing scripts (specified by shebang) with current python interpreter (sys.executable). This option is equivalent to appending 'build_scripts --executable PYTHON_EXECUTABLE' arguments to 'setup.py install' command.
    --python-disable-dependency python_package_name (python only) The python package name to remove from dependency list (default: [])
    --osxpkg-identifier-prefix IDENTIFIER_PREFIX (osxpkg only) Reverse domain prefix prepended to package identifier, ie. 'org.great.my'. If this is omitted, the identifer will be the package name.
    --[no-]osxpkg-payload-free    (osxpkg only) Define no payload, assumes use of script options. (default: false)
    --osxpkg-ownership OWNERSHIP  (osxpkg only) --ownership option passed to pkgbuild. Defaults to 'recommended'. See pkgbuild(1). (default: "recommended")
    --osxpkg-postinstall-action POSTINSTALL_ACTION (osxpkg only) Post-install action provided in package metadata. Optionally one of 'logout', 'restart', 'shutdown'.
    --osxpkg-dont-obsolete DONT_OBSOLETE_PATH (osxpkg only) A file path for which to 'dont-obsolete' in the built PackageInfo. Can be specified multiple times.
    --solaris-user USER           (solaris only) Set the user to USER in the prototype files. (default: "root")
    --solaris-group GROUP         (solaris only) Set the group to GROUP in the prototype file. (default: "root")
    --p5p-user USER               (p5p only) Set the user to USER in the prototype files. (default: "root")
    --p5p-group GROUP             (p5p only) Set the group to GROUP in the prototype file. (default: "root")
    --p5p-zonetype ZONETYPE       (p5p only) Set the allowed zone types (global, nonglobal, both) (default: "value=global value=nonglobal")
    --p5p-publisher PUBLISHER     (p5p only) Set the publisher name for the repository (default: "FPM")
    --[no-]p5p-lint               (p5p only) Check manifest with pkglint (default: true)
    --[no-]p5p-validate           (p5p only) Validate with pkg install (default: true)
    --freebsd-abi ABI             (freebsd only) Sets the FreeBSD abi pkg field to specify binary compatibility. (default: "Linux:3:x86_64")
    --freebsd-origin ABI          (freebsd only) Sets the FreeBSD 'origin' pkg field (default: "fpm/<name>")
    --virtualenv-pypi PYPI_URL    (virtualenv only) PyPi Server uri for retrieving packages. (default: "https://pypi.python.org/simple")
    --virtualenv-package-name-prefix PREFIX (virtualenv only) Name to prefix the package name with. (default: "virtualenv")
    --virtualenv-install-location DIRECTORY (virtualenv only) Location to which to install the virtualenv by default. (default: "/usr/share/python")
    --[no-]virtualenv-fix-name    (virtualenv only) Should the target package name be prefixed? (default: true)

--virtualenv-other-files-dir DIRECTORY (virtualenv only) Optionally, the
contents of the specified directory may be added to the package. This is useful
if the virtualenv needs configuration files, etc. (default: nil)

    --virtualenv-pypi-extra-url PYPI_EXTRA_URL (virtualenv only) PyPi extra-index-url for pointing to your priviate PyPi (default: nil)
    --pleaserun-name SERVICE_NAME (pleaserun only) The name of the service you are creating
    --pacman-optional-depends PACKAGE (pacman only) Add an optional dependency to the pacman package.
    --[no-]pacman-use-file-permissions (pacman only) Use existing file permissions when defining ownership and modes
    --pacman-user USER            (pacman only) The owner of files in this package (default: "root")
    --pacman-group GROUP          (pacman only) The group owner of files in this package (default: "root")
    --pacman-compression COMPRESSION (pacman only) The compression type to use, must be one of gz, bzip2, xz, none. (default: "xz")
    -h, --help                    print help

[kaynak:virtualenv icin kaynak](https://github.com/jordansissel/fpm/pull/930)
[github'i](ttps://github.com/jordansissel/fpm)
