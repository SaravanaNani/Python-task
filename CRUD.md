# Python CRUD Operations to Upload Files to GCS

### Prequisites:

1. Google Cloud Project: You need a GCP project.
2. Service Account: Ensure you have a service account with Storage Object Admin or Storage Object Creator role to interact with GCS.
3. Google Cloud SDK or Cloud Shell: Ensure gcloud is installed and authenticated on your machine or use Cloud Shell
Steps:

# Open Google Cloud Shell:

-> Go to your Google Cloud Console (https://console.cloud.google.com).

-> Click on the Cloud Shell icon (a terminal window in the top-right corner).

Install Required Libraries

-> You'll need the Google Cloud Storage client library to interact with GCS. You can install it using

```
pip install google-cloud-storage

```
-> Create a Folder for Your Script:

In the Cloud Shell terminal, run the following command to create a new directory:
```
mkdir storage-bk.
cd storage-bk.py
```
### To Create and Edit the Python Script:

-> Use the Cloud Shell editor to create a Python file (bk.py).

-> Click on Open Editor in the Cloud Shell.

-> In the left panel, right-click on the ```storage-bk``` folder and select New File.

-> Name the ```file bk.py ```and copy-paste your Python code into it.

### TO Create and Upload the Service Account Key:

-> In the same folder ```(storage-bk)```, create another file named ```key.json```.

-> Copy and paste the content of your service account key into this file.

### Run the Python Script:

Back in the Cloud Shell terminal, navigate to your folder:

```
cd storage-bk
```
### Run the Python script using the following command:

```
python bk.py
```
#### 1.  Create-new-storage-bucket


```
from google.cloud import storage
import os

os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "key1.json"

project_id = '<YOUR_PROJECT_ID>'
bucket_name = '<GIVE_BUCKET_NAME>'

storage_client = storage.Client()
bucket = storage_client.bucket(bucket_name)
bucket.location = 'us'
bucket.create(project=project_id,location="us")
print(f'Bucket {bucket_name} created.')

```
Here in bucket.location = 'us' , it means youn are creating multi-regional bucket in "us region"

### 2. Create-new-object-in-already-existing-bucket-with-your-data:
```
from google.cloud import storage
import os

# Set the environment variable for authentication using the service account key
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "key.json"  # Path to your service account key

project_id = 'your-project-id'        # Replace with your Google Cloud project ID
bucket_name = 'your-new-bucket-name'  # Replace with the desired name of your new bucket

storage_client = storage.Client(project=PROJECT_ID)

# Define the object name (including any path within the bucket)
object_name = "YOUR_OBJECT_NAME"  # Replace with your desired filename

# Prepare the data you want to upload (can be string, bytes, or a file object)

data = "This is some text data to upload."  # Replace with your actual data

blob = storage_client.bucket(BUCKET_NAME).blob(object_name)

blob.upload_from_string(data)

print(f"Object '{object_name}' uploaded to bucket '{BUCKET_NAME}'.")

```
### 3. Creating-new-bucket-with-object & copying-data-from-local-file-to-created-object

```
from google.cloud import storage
import os

# Set the environment variable for authentication using the service account key
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "key.json"  # Path to your service account key

project_id = 'your-project-id'  # Replace with your Google Cloud project ID
bucket_name = 'your-new-bucket-name'  # Replace with the desired name of your new bucket

# Initialize the Google Cloud Storage client
storage_client = storage.Client()

# Create a new bucket in the specified project and location (us in this case)
bucket = storage_client.bucket(bucket_name)
bucket.location = 'us'
bucket.create(project=project_id, location="us")
print(f'Bucket {bucket_name} created.')

# Upload an object to the newly created bucket
blob = bucket.blob("your_object_name")  # Replace with the object name (file name) in the bucket
with open('your_source_file_name', 'rb') as file:  # Replace with the local file path to upload
    blob.upload_from_file(file)
print(f'File uploaded to {bucket_name} as {blob.name}.')
```

### 4. List-objects-in-bucket

```
from google.cloud import storage

def list_files(bucket_name):
    # Create a client to interact with Google Cloud Storage
    client = storage.Client()

    # Get the specified bucket
    bucket = client.get_bucket(bucket_name)

    # List all files in the bucket
    blobs = bucket.list_blobs()

    # Print the names of the files
    print("Files in bucket '{}':".format(bucket_name))
    for blob in blobs:
        print(blob.name)

# Replace 'your_bucket_name' with the actual name of your GCS bucket
list_files('your_bucket_name')  # Replace with the actual bucket name
```
### 5. Delete-object-from-bucket

```
from google.cloud import storage

def delete_object(bucket_name, object_name):

    # Create a client to interact with Google Cloud Storage
    client = storage.Client()

    # Get the specified bucket
    bucket = client.get_bucket(bucket_name)

    # Get the specified object (file)
    blob = bucket.blob(object_name)

    # Delete the object
    blob.delete()

    print("Object '{}' in bucket '{}' deleted successfully.".format(object_name, bucket_name))

# Replace 'your_bucket_name' with the actual name of the GCS bucket
# Replace 'your_object_name' with the name of the object you want to delete

delete_object('your_bucket_name', 'your_object_name')  # Provide the correct bucket and object names

```
### 6. Delete-all-objects-in-bucket 

```
from google.cloud import storage

def delete_objects(bucket_name):
    # Create a client to interact with Google Cloud Storage
    client = storage.Client()

    # Get the specified bucket
    bucket = client.get_bucket(bucket_name)

    # List all objects in the bucket
    blobs = bucket.list_blobs()

    # Delete each object in the bucket
    for blob in blobs:
        blob.delete()

    print("All objects in bucket '{}' deleted successfully.".format(bucket_name))

# Replace 'your_bucket_name' with the actual name of the GCS bucket
delete_objects('your_bucket_name')  # Provide the correct bucket name

```
### 7. Delete-Bucket

```
from google.cloud import storage

def delete_bucket(bucket_name):

    # Create a client to interact with Google Cloud Storage
    client = storage.Client()

    # Get the specified bucket
    bucket = client.get_bucket(bucket_name)

    # Delete the bucket
    bucket.delete(force=True)

    print("Bucket '{}' deleted successfully.".format(bucket_name))

# Replace 'your_bucket_name' with the actual name of the GCS bucket you want to delete

delete_bucket('your_bucket_name')  # Provide the correct bucket name

```
