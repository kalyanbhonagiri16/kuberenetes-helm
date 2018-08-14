Edit values.yml for ports or any changes and
run $ helm install --name example ./apache2

you can either set variables in files using
$ helm install --name example ./mychart --set service.type=NodePort
