- meta:
    - topics: [[DMS]] [[selfhosted]] [[published posts]]

***

# SeedDMS

This is a tutorial on how to install SeedDMS 4.3 on [[Debian]] Jessie with
an external MySQL server.

First we have to install all the depencies:

```bash
sudo apt-get install apache2 php5 php5-mysql php5-gd php-pear imagemagick poppler-utils catdoc
```

Second we have to install the required pear packages:

```bash
sudo pear install Log
sudo pear channel-discover pear.dotkernel.com/zf1/svn
sudo pear install zend/zend
```

To have it fully working we need to enable the rewrite module for
[[Apache]]:

```bash
sudo a2enmod rewrite
```

In addition we need change the [[Apache]] config. There should be a part in
the /etc/apache2/apache2.conf which looks like this:

```bash
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

We need to change it to this:

```bash
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```

Now we can download the SeedDMS files from here:
https://sourceforge.net/projects/seeddms/files

For this installation I went with seeddms-quickstart-4.3.28.tar.gz. I've
downloaded it to _var/www_ and extracted the archive. Inside the now
generated folder there are 4 folders however one of them is only a
symlink:

- data
- pear
- seeddms-4.3.28
- www (symlink to seeddms-4.3.28)

For my installation I've moved the data folder to _var_ so that it isn't
directly reachable by the webserver. Then I moved the seeddms-4.3.28
folder to _var/www_ and renamed it to html

```bash
sudo mv /var/www/seeddms43x/seeddms-4.3.28 /var/www/html
```

Now I moved the pear folder into the html folder and copied the SeedDMS
folder inside the pear folder into the html folder. This is required
because it was complaining that it couldn't find the path otherwise.

```bash
sudo mv /var/www/seeddms43x/pear/ /var/www/html/
```

```bash
sudo cp /var/www/html/pear/SeedDMS /var/www/html/
```

Don't forget to give all the folders read/write permissions for the user
www-data.

As a last step we have to change the configuration file
(/var/www/html/conf/settings.xml) to reflect our changes. It would be a
bit much to cover all the changes necessary. Just make sure that all the
paths we've changed are correct. The config file itself is very well
commented so it shouldn't be a big problem to find all the places
requiring a correction. I'll paste however the lines how they look on my
system:

```bash
<server rootDir="/var/www/html/" httpRoot="/" contentDir="/var/data/" stagingDir="/var/data/staging/" luceneDir="/var/data/lucene/" logFileEnable="true" logFileRotation="d" enableLargeFileUpload="false" partitionSize="2000000" cacheDir="/var/data/cache/" dropFolderDir="">
   </server>
```

```bash
<database dbDriver="mysql" dbHostname="mariadb.2li.local" dbDatabase="seeddmsdb" dbUser="seeddms" dbPass="Password" doNotCheckVersion="false">
  </database>
```

Now restart the [[Apache]] service and you should be able to access the
installation on http://dms-ip

## Resources

- https://myanwyn.blogspot.ch/2014/12/how-to-install-seeddms-on-ubuntu.html
