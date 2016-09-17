# Plugins
List of SocketCluster plugins/modules.

If you've built a frontend or backend plugin for SocketCluster, feel free to make a PR and share it with the community.
The idea is that this repo will eventually turn into "a marketplace for SocketCluster plugins".

## Open source plugins

**Frontend**

- ```sc-field``` https://github.com/SocketCluster/sc-field - A Polymer component which represents a realtime auto-updating data field from ```sc-crud-rethink```.
- ```sc-collection``` https://github.com/SocketCluster/sc-collection - A Polymer component which represents a realtime auto-updating collection from ```sc-crud-rethink```.
- ```vue-socketcluster``` https://github.com/happilymarrieddad/vue-socketcluster - A Vue component which integrates socketcluster-client directly into VueJS.

**Backend**

- ```sc-crud-rethink``` https://github.com/SocketCluster/sc-crud-rethink - A Create Read Update Delete (CRUD) module/layer to allow you to quickly write "server-less" realtime apps using SC.

- ```sc-crud-mysql``` https://github.com/happilymarrieddad/sc-crud-mysql - A Create Read Update Delete (CRUD) module/layer to allow you to quickly write "server-less" realtime apps using SC and MySQL.

## Enterprise service plugins

If you have an idea for a plugin which will deliver a service around SC (it doesn't have to be a free service), get in touch with me (https://github.com/jondubois) and
I might list it here as an official plugin (if it meets quality standards). This should allow you to quickly build a customer base from existing SC users.

**Backend**

- Coming soon: A service plugin to allow realtime monitoring/analysis of pub/sub channels within SC.

## Contribution guidelines

- Make sure your plugin has a Github repo and a README.md file.
- The name of your plugin (both frontend and backend) should start with ```sc-```
- Look at existing plugins above and try to follow conventions were possible.

**Frontend**

- Publish to bower.
- Other than that, it's whatever works; just keep it clean and documented!

**Backend**

- Publish to npm

Your backend plugin should expose an ```attach(scObject[, options, callback])``` method - The scObject argument should be an instance of one of the following
classes: SocketCluster, SCWorker, SCServer, SCSocket or Broker.
Right now, the user will have to attach your plugin manually to the appropriate scObject, but there are plans to automate this so that users will be able to
install your plugin using a ```socketcluster install mypluginname``` command.
The class of the scObject depends on which part of SC's architecture are affected:

If attached from master process ```server.js```, scObject will be:

- SocketCluster http://socketcluster.io/#!/docs/api-socketcluster


If attached from workerController ```worker.js```, scObject will be one of:

- SCWorker http://socketcluster.io/#!/docs/api-scworker
- SCServer http://socketcluster.io/#!/docs/api-scserver
- SCSocket http://socketcluster.io/#!/docs/api-scsocket-server

If attached from brokerController ```broker.js```, scObject will be:

- Broker http://socketcluster.io/#!/docs/api-broker

Internally, your plugin should extend the functionality of the scObject provided.

You should use private events instead of public ones (where possible) to prevent the user from interfering
with your plugin's logic.
For example, use the '_handshake', '_connection', '_disconnection' and '_disconnect' events
instead of their public counterparts 'handshake', 'connection', 'disconnection' and 'disconnect'...
