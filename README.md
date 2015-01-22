Vane

Vane is a GPL fork of the now non-free popular wordpress vulnerability scanner WPScan.

Backstory: https://www.delvelabs.ca/robbed-gunpoint/


Please note that the WPScan team does not bear any responsibility for anything in this
program.

==LICENSE==

Vane - A WordPress vulnerability scanner

Copyright (C) 2012-2015 WPScan Team
Copyright (C) 2015 Delve Labs inc.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.


==INSTALL==

  Prerequisites:

   * Windows not supported
   * Ruby => 1.9
   * RubyGems
   * Git

  -> Installing on Debian/Ubuntu:

    sudo apt-get install libcurl4-gnutls-dev libopenssl-ruby libxml2 libxml2-dev libxslt1-dev ruby-dev
    git clone https://github.com/delvelabs/vane.git
    cd vane
    sudo gem install bundler && bundle install --without test development

  -> Installing on Fedora:

    sudo yum install libcurl-devel
    git clone https://github.com/delvelabs/vane.git
    cd vane
    sudo gem install bundler && bundle install --without test development

  -> Installing on Archlinux:

    pacman -Sy ruby
    pacman -Sy libyaml

    git clone https://github.com/delvelabs/vane.git
    cd vane
    sudo gem install bundler && bundle install --without test development

    gem install typhoeus
    gem install nokogiri

  -> Installing on Mac OS X:

    git clone https://github.com/delvelabs/vane.git
    cd vane
    sudo gem install bundler && bundle install --without test development

==KNOWN ISSUES==

  - Typhoeus segmentation fault:
      Update cURL to version => 7.21 (may have to install from source)
      See http://code.google.com/p/vane/issues/detail?id=81

  - Proxy not working:
      Update cURL to version => 7.21.7 (may have to install from source).

      Installation from sources :
        - Grab the sources from http://curl.haxx.se/download.html
        - Decompress the archive
        - Open the folder with the extracted files
        - Run ./configure
        - Run make
        - Run sudo make install
        - Run sudo ldconfig

  - cannot load such file -- readline:
      Run sudo aptitude install libreadline5-dev libncurses5-dev

      Then, open the directory of the readline gem (you have to locate it)

      cd ~/.rvm/src/ruby-1.9.2-p180/ext/readline
      ruby extconf.rb
      make
      make install

      See http://vvv.tobiassjosten.net/ruby-on-rails/fixing-readline-for-the-ruby-on-rails-console/ for more details



==WPSCAN ARGUMENTS==

--update   Update to the latest revision

--url   | -u <target url>  The WordPress URL/domain to scan.

--force | -f Forces WPScan to not check if the remote site is running WordPress.

--enumerate | -e [option(s)]  Enumeration.
  option :
    u        usernames from id 1 to 10
    u[10-20] usernames from id 10 to 20 (you must write [] chars)
    p        plugins
    vp       only vulnerable plugins
    ap       all plugins (can take a long time)
    tt       timthumbs
    t        themes
    vp       only vulnerable themes
    at       all themes (can take a long time)
  Multiple values are allowed : '-e tt,p' will enumerate timthumbs and plugins
  If no option is supplied, the default is 'vt,tt,u,vp'

--exclude-content-based '<regexp or string>'  Used with the enumeration option, will exclude all occurrences based on the regexp or string supplied
                                              You do not need to provide the regexp delimiters, but you must write the quotes (simple or double)

--config-file | -c <config file> Use the specified config file

--follow-redirection  If the target url has a redirection, it will be followed without asking if you wanted to do so or not

--wp-content-dir <wp content dir>  WPScan try to find the content directory (ie wp-content) by scanning the index page, however you can specified it. Subdirectories are allowed

--wp-plugins-dir <wp plugins dir>  Same thing than --wp-content-dir but for the plugins directory. If not supplied, WPScan will use wp-content-dir/plugins. Subdirectories are allowed

--proxy <[protocol://]host:port>  Supply a proxy (will override the one from conf/browser.conf.json).
                                  HTTP, SOCKS4 SOCKS4A and SOCKS5 are supported. If no protocol is given (format host:port), HTTP will be used

--proxy-auth <username:password>  Supply the proxy login credentials (will override the one from conf/browser.conf.json).

--basic-auth <username:password>  Set the HTTP Basic authentication

--wordlist | -w <wordlist>  Supply a wordlist for the password bruter and do the brute.

--threads  | -t <number of threads>  The number of threads to use when multi-threading requests. (will override the value from conf/browser.conf.json)

--username | -U <username>  Only brute force the supplied username.

--help     | -h This help screen.

--verbose  | -v Verbose output.

==WPSCAN EXAMPLES==

Do 'non-intrusive' checks...

  ruby vane.rb --url www.example.com

Do wordlist password brute force on enumerated users using 50 threads...

  ruby vane.rb --url www.example.com --wordlist darkc0de.lst --threads 50

Do wordlist password brute force on the 'admin' username only...

  ruby vane.rb --url www.example.com --wordlist darkc0de.lst --username admin

Enumerate installed plugins...

  ruby vane.rb --url www.example.com --enumerate p

==VANETOOLS ARGUMENTS==

--help    | -h   This help screen.
--Verbose | -v   Verbose output.
--update  | -u   Update to the latest revision.
--generate_plugin_list [number of pages]  Generate a new data/plugins.txt file. (supply number of *pages* to parse, default : 150)
--gpl  Alias for --generate_plugin_list
--check-local-vulnerable-files | --clvf <local directory>  Perform a recursive scan in the <local directory> to find vulnerable files or shells

==VANETOOLS EXAMPLES==

- Generate a new 'most popular' plugin list, up to 150 pages ...
ruby vanetools.rb --generate_plugin_list 150

- Locally scan a wordpress installation for vulnerable files or shells :
ruby vanetools.rb --check-local-vulnerable-files /var/www/wordpress/

