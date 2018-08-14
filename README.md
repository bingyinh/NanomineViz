# NanomineViz
visualization for nanomine project

# Installation (REQUIRES ubuntu 16.04 -- 18.04 will not work yet)
### Note: if installing on a virtual machine, be sure to allocate at least 8G Memory, 2 CPUs and 50G Disk
  ```
  bash < <(curl -skL https://raw.githubusercontent.com/tetherless-world/whyis/master/install.sh)
  ```
- whyis will be installed in /apps/whyis
- install NanomineViz app following:
  ```
  cd /apps
  sudo git clone https://github.com/Duke-MatSci/NanomineViz.git
  cd /apps/NanomineViz/data
  sudo wget https://raw.githubusercontent.com/Duke-MatSci/nanomine-ontology/master/xml_ingest.setl.ttl
  sudo wget https://raw.githubusercontent.com/Duke-MatSci/nanomine-ontology/master/ontology.setl.ttl
  sudo chown -R whyis:whyis /apps/NanomineViz
  sudo su - whyis
  cd /apps/NanomineViz
  pip install -e .
  exit
  sudo service apache2 restart
  sudo service celeryd restart
  sudo su - whyis
  cd /apps/whyis
  python manage.py createuser -e (email) -p (password) -f (frstname) -l (lastname) -u (username) --roles=admin
  python manage.py load -i /apps/NanomineViz/data/viz.ttl -f turtle
  cd /apps
  touch .netrc
  ```
- after you create the .netrc file under /apps, you need to edit this file.
  ```
  machine some_url (like 000.000.000.000 or nanomine.oit.duke.edu) -- this is the address of the nanomine server
  login some_username (like testuser1)
  password some_password (like testpwd1)
  ```
- after you edit the .netrc file, in your terminal type:
  - the xml_ingest.setl.ttl file references the nanomine server protocol, address and port and may need to  be edited
  ```
  cd /apps/whyis
  python manage.py load -i /apps/NanomineViz/data/ontology.setl.ttl -f turtle
  python manage.py load -i /apps/NanomineViz/data/xml_ingest.setl.ttl -f turtle
  ```
  - The load process can be monitored with 'sudo tail -f /var/log/celery/w1.log'
- go to http://localhost/ to login with your credentials during "createuser" command
- go to http://localhost/viz to access the visualization

# Developing mode
Each time a change is made on the visualization, apache2 and celeryd service have to be restarted manually. 
This is very troublesome. whyis has a developing mode that help you allevate this pain. 
```
sudo su - whyis
cd /app/whyis
python manage.py runserver -h 0.0.0.0
``` 
The visualization then will be accessed on "http://localhost:5000/viz".
Then, you only need to refresh your webpage to see your changes immediately after you make changes to the visualization. 
After you finished the visualization changes, you can shutdown the developing mode with CTRL+c.
Then you have to restart apache2 and celeryd service by
```
sudo service apache2 restart
sudo service celeryd restart
```
After this, the updated visualization app will show up at "http://localhost/viz".
