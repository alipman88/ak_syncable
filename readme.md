ak_syncable
=========

A module to easily sync any Rails model to ActionKit

```sh
gem 'ak_syncable', :git => 'git://github.com/Democracy-for-America/ak_syncable.git'
```

Example
---------
```sh
class Foo < ActiveRecord::Base

  include AkSyncable

  @@synced_attributes = [:name, :email, :zip, :comment]

  syncs_to "name_of_actionkit_page_to_sync_to"

  after_save :sync_to_actionkit

end
```

Options
---------
@@synced_attributes
```sh
# set the attributes that get synced to ActionKit with a class variable, @@synced_attributes

@@synced_attributes = [:name, :email, :zip, :comment]
# sets the attributes to sync to ActionKit -- any attribute that isn't an ActionKit core_user field will be prefixed with action_ and synced as a core_actionfield

# avilable default fields include
#   :address1, :address2, :city, :country, :email, :first_name, :last_name, :middle_name, :name, :phone, :postal,:prefix, :region, :state, :suffix, :zip, :source, :akid, :referring_akid
# other fields, e.g. :comment may be added to the @@synced_attributes array -- they will be prefixed with "action_" and synced as core_actionfield entries
```
syncs_to
```sh
set the name of the page that will be synced to with the syncs_to() class method

# syncs_to can accept a string:
syncs_to "YOUR_ACTIONKIT_PAGE_NAME_HERE" 

# or a symbol/method:
syncs_to :actionkit_page
def actionkit_page
  return self.region.actionkit_namespace + '_nominations'
end

# or a lambda:
syncs_to -> { self.region.actionkit_namespace + '_nominations' }
```

sync_to_actionkit
```sh
# call syncs_to_actionkit on an instance of any AkSyncable class, to sync that instance to ActionKit

# e.g.
@foo.sync_to_actionkit

# to utilize DelayedJob to process ActionKit syncing as a background task (recommended),
# simply add the delayed_job gem, and add the following line to the synced classes:
handle_asynchronously :sync_to_actionkit
```

Important
---------
Don't forget to set the envirnoment variables!
* ENV['ACTIONKIT_PATH'] # (e.g. 'https://roboticdogs.actionkit.com/rest/v1/')
* ENV['ACTIONKIT_USERNAME'] # (an API-enabled ActionKit user)
* ENV['ACTIONKIT_PASSWORD']

Requirements
---------
* httparty

License
----
MIT
