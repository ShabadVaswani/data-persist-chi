
::: {.cell .markdown}

## Delete resources

When we are finished, we must delete 

* the VM server instance 
* the block storage volume
* and the object store container

to make the resources available to other users.

We will execute the cells in this notebook inside the Chameleon Jupyter environment.

Run the following cell, and make sure the correct project is selected. 

:::

::: {.cell .code}
```python
# run in Chameleon Jupyter environment
from chi import server, context
import chi, os, time, datetime

context.version = "1.0" 
context.choose_project()
context.choose_site(default="KVM@TACC")
```
:::


::: {.cell .markdown}

Delete the compute instance:

:::


::: {.cell .code}
```python
# run in Chameleon Jupyter environment
username = os.getenv('USER') # all exp resources will have this prefix
s = server.get_server(f"node-persist-{username}")
s.delete()
```
:::

::: {.cell .markdown}

Wait a moment for this operation to be finished before you try to delete the block storage volume - you can't delete the volume when it is attached to a running instance.
:::



::: {.cell .markdown}

Delete the block storage volume - in the following cell, substitute your own net ID in place of **netID**:

:::


::: {.cell .code}
```python
# run in Chameleon Jupyter environment
cinder_client = chi.clients.cinder()
volume = [v for v in cinder_client.volumes.list() if v.name=='block-persist-netID'][0] # Substitute your own net ID
cinder_client.volumes.delete(volume = volume)
```
:::


::: {.cell .markdown}

And finally, delete the object store container at CHI@TACC. We will use the OpenStack Swift client to delete all the objects, and then the container. 

:::


::: {.cell .code}
```python
# run in Chameleon Jupyter environment
context.choose_project()
context.choose_site(default="CHI@TACC")
```
:::

::: {.cell .code}
```python
# run in Chameleon Jupyter environment
os_conn = chi.clients.connection()
token = os_conn.authorize()
storage_url = os_conn.object_store.get_endpoint()

import swiftclient
swift_conn = swiftclient.Connection(preauthurl=storage_url,
                                    preauthtoken=token,
                                    retries=5)
```
:::

::: {.cell .markdown}

In the following cell, replace **netID** with your own net ID: 
:::


::: {.cell .code}
```python
# run in Chameleon Jupyter environment
container_name = "object-persist-netID"
while True:
    _, objects = swift_conn.get_container(container_name, full_listing=True)
    if not objects:
        break
    paths = "\n".join(f"{container_name}/{obj['name']}" for obj in objects)
    swift_conn.post_account(
        headers={"Content-Type": "text/plain"},
        data=paths,
        query_string="bulk-delete"
    )
swift_conn.delete_container(container_name)
print("Container deleted.")
```
:::
