
```
public void startAllScan(final Context context, final Scanner sc) {
        BluetoothAdapter bla = BluetoothAdapter.getDefaultAdapter();
        if (!bla.isEnabled()) {
            bla.enable();
        }
        Toast.makeText(context, "Bluetooth Devices Fetching started!!", Toast.LENGTH_LONG).show();
        bla.startDiscovery();
        final BroadcastReceiver mReceiver = new BroadcastReceiver() {
            public void onReceive(Context context, Intent intent) {
                String action = intent.getAction();

                //Finding devices
                if (BluetoothDevice.ACTION_FOUND.equals(action)) {
                    // Get the BluetoothDevice object from the Intent
                    BluetoothDevice device = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE);
                    // Add the name and address to an array adapter to show in a ListView
                    if (!mRecordedResults.contains(device.getAddress())) {
                        System.out.println(device.getAddress());
                        mRecordedResults.add(device.getAddress());
                    }
                }
            }
        };

        IntentFilter filter = new IntentFilter(BluetoothDevice.ACTION_FOUND);
        context.registerReceiver(mReceiver, filter);

        new CountDownTimer(60000, 30000) {

            @Override
            public void onTick(long l) {
                System.out.println("On Tick");
            }

            @Override
            public void onFinish() {
                System.out.println("On Finish");
                context.unregisterReceiver(mReceiver);
                sc.scanningComplete(mRecordedResults);
            }
        }.start();
    }
```

```
public interface Scanner {
        void scanningComplete(List<String> listOfAddress);
    }
```


Get current device bluetooth address
```
public static String getBluetoothAddress() {
        if (Build.VERSION_CODES.M > VERSION.SDK_INT) {
            BluetoothAdapter bla = BluetoothAdapter.getDefaultAdapter();
            if (!bla.isEnabled()) {
                bla.enable();
            }
            return bla.getAddress();
        } else {
            String output = "";
            try {
                List<NetworkInterface> all = Collections.list(NetworkInterface.getNetworkInterfaces());
                for (NetworkInterface nif : all) {
                    if (!nif.getName().equalsIgnoreCase("wlan0")) continue;

                    byte[] macBytes = nif.getHardwareAddress();
                    if (macBytes == null) {
                        output = "";
                    }

                    StringBuilder res1 = new StringBuilder();
                    for (byte b : macBytes) {
                        res1.append(String.format("%02X:", b));
                    }

                    if (res1.length() > 0) {
                        res1.deleteCharAt(res1.length() - 1);
                    }
                    output = res1.toString();
                }
            } catch (Exception ex) {
            }
            return output;
        }
    }
```
