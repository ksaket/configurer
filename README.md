Configurer
==========

Provides a way to simplify configuration management on Google App Engine applications (Java).

=========== Some basic notes ===================== :

A distributed configuration management for GAE application. Idea is to place configurations in cloud storage and poll for changes. Configurations are parsed and 	stored in Datastore and primarily served from Memcache. 

(Future Roadmap) - Configurations are also stored in local cache per instance using a LRU queue to avoid API call latencies. A flag is placed in an instance which is dirtied with configuration changes.

Application exposes a handler that allows Admin users to upload a JSON file to cloud storage which is subsequently parsed and stored in Datastore and Memcache. (Also look into cloud storage object change notification). Additionally, Admin can also specify an existing Cloud Storage file location to kickoff the task post deployment.

Configurations can be set at different levels:

 			App ---|
				   |
			      Module ---|
							|
						 Version ---|
									|
								Namespace



As configuration files can be large in size, an streaming API should be used to read and parse configurations. N (Configurable) number of backups should be maintained and it should be easy to revert configurations if necessary. AuditLog of every config change should also be maintained.


Config location - gs://{app-id}/configurations/app-config.json

Design:

										--------------
					---------------->  | Cloud Storage | ---------------	
					|					--------------                 |
					|                                                  | Parse/Verify/LogAudit config file
					|                                                  |
					|												   V	
	  ---------------------------------   RPC through URLFetch    -------------    Store          ---------------
     | GAE Frontend App Config Handler |  -------------------->  | GAE Backend |  -------------> | GAE Datastore |
      ---------------------------------                           -------------                   ---------------
																		|								|
																		| Store							|
																		V								|
																  --------------                        |
																 | GAE Memcache |                       |
																  --------------                        |
																	    |                               |
																		|                               |
																		V                               |
																  -------------      Failover to        |
																 | Application | <----------------------|
																  -------------
																

TODO - Add libraries and data structures to be used.
