# Alpine Linux with s6 service management
FROM smebberson/alpine-base

# Install Apache2 and other stuff needed to access svn via WebDav
RUN apk add --no-cache apache2 apache2-utils apache2-webdav mod_dav_svn &&\
	# Install svn
	apk add --no-cache subversion &&\

	# Create required folders
	mkdir -p /run/apache2/ &&\
	mkdir /home/svn/ &&\
	mkdir /etc/subversion &&\

	# Create the authentication file for http access
	touch /etc/subversion/passwd

# Add WebDav configuration
ADD dav_svn.conf /etc/apache2/conf.d/dav_svn.conf

# Add SVNAuth file
ADD subversion-access-control /etc/subversion/subversion-access-control
RUN chown -R apache:apache /home/svn/

CMD [ "/usr/bin/svnserve", "--daemon", "--foreground", "--root", "/var/opt/svn", "--listen-port", "3960" ]
CMD [ "/usr/sbin/apachectl", "-DFOREGROUND" ]

# Expose ports for http and custom protocol access
EXPOSE 80 443 3960