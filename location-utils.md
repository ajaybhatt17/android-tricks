```
//For Coarse Location in API Level 23 and above (Running Permission)
if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
         ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_COARSE_LOCATION}, REQUEST_CODE);
} 
```

```
//For Fine Location Permission in API Level 23 and above (Running Permission)
if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
         ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, REQUEST_CODE);
}
```

```
//Put this callback function in activity in which you want to take user location
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String permissions[], @NonNull int[] grantResults) {
     switch (requestCode) {
         case REQUEST_CODE: {
             if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    //Action Trigger on Getting Location Access
                } else {
                    if (!shouldShowRequestPermissionRationale(Manifest.permission.ACCESS_FINE_LOCATION)) {
                        /* User has selected "NEVER ASK AGAIN" for runtime location permission*/
                        // In this case show user that without this permission functionality wouldnt work properly through toast or dialog
                    } else
                        // In this case User opt for not allowing ... so show user where & why we need location permission through toast or dialog
                }
            }
        }
    }
```
