# Unknown Storage

**Date:** 28, May, 2021

**Author:** Dhilip Sanjay S

---

### What is the name of the file you found?
- **Answer:** employee_names.txt 
- **Steps to Reproduce:** 
    - Visit `https://advent-bucket-one.s3.amazonaws.com/` in browser.
    - You'll get the following **XML response**.

```xml
<ListBucketResult>
<Name>advent-bucket-one</Name>
<Prefix/>
<Marker/>
<MaxKeys>1000</MaxKeys>
<IsTruncated>false</IsTruncated>
<Contents>
<Key>employee_names.txt</Key>
<LastModified>2019-12-14T15:53:25.000Z</LastModified>
<ETag>"e8d2d18588378e0ee0b27fa1b125ad58"</ETag>
<Size>7</Size>
<StorageClass>STANDARD</StorageClass>
</Contents>
<Contents>
<Key>test.txt</Key>
<LastModified>2021-04-02T18:21:13.000Z</LastModified>
<ETag>"098f6bcd4621d373cade4e832627b4f6"</ETag>
<Size>4</Size>
<Owner>
<ID>65a011a29cdf8ec533ec3d1ccaae921c</ID>
</Owner>
<StorageClass>STANDARD</StorageClass>
</Contents>
</ListBucketResult>
```

{% hint style="info" %}
Once we have an s3 bucket, we can check if it’s publicly accessible by browsing to the URL. The format of the URL is: `bucketname.s3.amazonaws.com`
{% endhint %}

---

### What is in the file?
- **Answer:** mrchef
- **Steps to Reproduce:** Visit `https://advent-bucket-one.s3.amazonaws.com/employee_names.txt` in browser.

---