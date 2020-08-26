# Repo that aims at reverse-engineering the TTLock protocol

## The basics

The Bluetooth lock name consists of the last 6 MAC digits, in inverted order. For instance, the lock with the name "S202F_abcdef" has the MAC "ee:c6:a2:**ef**:**cd**:**ab**".

### Getting all primary UUIDs with gatttool:

```
attr handle: 0x0001, end grp handle: 0x0007 uuid: 00001800-0000-1000-8000-00805f9b34fb
attr handle: 0x0008, end grp handle: 0x000b uuid: 00001801-0000-1000-8000-00805f9b34fb
attr handle: 0x000c, end grp handle: 0x0011 uuid: 00001910-0000-1000-8000-00805f9b34fb
attr handle: 0x0012, end grp handle: 0x0015 uuid: 0000180f-0000-1000-8000-00805f9b34fb
attr handle: 0x0016, end grp handle: 0x001e uuid: 0000180a-0000-1000-8000-00805f9b34fb
attr handle: 0x001f, end grp handle: 0xffff uuid: 00001530-1212-efde-1523-785feabcd123
```

### Getting all available handles with gatttool:

```
handle: 0x0001, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x0002, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0003, uuid: 00002a00-0000-1000-8000-00805f9b34fb
handle: 0x0004, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0005, uuid: 00002a01-0000-1000-8000-00805f9b34fb
handle: 0x0006, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0007, uuid: 00002a04-0000-1000-8000-00805f9b34fb
handle: 0x0008, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x0009, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x000a, uuid: 00002a05-0000-1000-8000-00805f9b34fb
handle: 0x000b, uuid: 00002902-0000-1000-8000-00805f9b34fb
handle: 0x000c, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x000d, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x000e, uuid: 0000fff4-0000-1000-8000-00805f9b34fb *
handle: 0x000f, uuid: 00002902-0000-1000-8000-00805f9b34fb
handle: 0x0010, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0011, uuid: 0000fff2-0000-1000-8000-00805f9b34fb *
handle: 0x0012, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x0013, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0014, uuid: 00002a19-0000-1000-8000-00805f9b34fb *
handle: 0x0015, uuid: 00002902-0000-1000-8000-00805f9b34fb
handle: 0x0016, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x0017, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0018, uuid: 00002a29-0000-1000-8000-00805f9b34fb
handle: 0x0019, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x001a, uuid: 00002a24-0000-1000-8000-00805f9b34fb
handle: 0x001b, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x001c, uuid: 00002a27-0000-1000-8000-00805f9b34fb
handle: 0x001d, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x001e, uuid: 00002a26-0000-1000-8000-00805f9b34fb
handle: 0x001f, uuid: 00002800-0000-1000-8000-00805f9b34fb
handle: 0x0020, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0021, uuid: 00001532-1212-efde-1523-785feabcd123
handle: 0x0022, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0023, uuid: 00001531-1212-efde-1523-785feabcd123
handle: 0x0024, uuid: 00002902-0000-1000-8000-00805f9b34fb
handle: 0x0025, uuid: 00002803-0000-1000-8000-00805f9b34fb
handle: 0x0026, uuid: 00001534-1212-efde-1523-785feabcd123

* Some kind of knowledge about what it does, explained below
```

### Handle descriptions

#### `00002a19-0000-1000-8000-00805f9b34fb` [READ]

- Battery percentage, 1 byte

#### `0000fff2-0000-1000-8000-00805f9b34fb` [WRITE NO-RESP]

- Appears to handle exchange of keys to open the lock, after the exchange of data, the lock opens

#### `0000fff4-0000-1000-8000-00805f9b34fb` [NOTIFY]

- The TTLock Official App seems to subscribe to this in order to get back if the lock was opened
