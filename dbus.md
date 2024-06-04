### How to to know services publishing on DBUS?

##### Verify object tree of all services:
`
$ busctl tree
`

##### Verfiy object tree for given service:
`
busctl tree org.freedesktop.NetworkManager
`

##### Introspect given object:
`
busctl introspect org.freedesktop.NetworkManager /org/freedesktop/NetworkManager/IP4Config/1
`

Example result:

```
NAME                                     TYPE      SIGNATURE RESULT/VALUE                             FLAGS
org.freedesktop.DBus.Introspectable      interface -         -                                        -
.Introspect                              method    -         s                                        -
org.freedesktop.DBus.Peer                interface -         -                                        -
.GetMachineId                            method    -         s                                        -
.Ping                                    method    -         -                                        -
org.freedesktop.DBus.Properties          interface -         -                                        -
.Get                                     method    ss        v                                        -
.GetAll                                  method    s         a{sv}                                    -
.Set                                     method    ssv       -                                        -
.PropertiesChanged                       signal    sa{sv}as  -                                        -
org.freedesktop.NetworkManager.IP4Config interface -         -                                        -
.AddressData                             property  aa{sv}    1 2 "address" s "192.168.1.25" "prefix"… emits-change
.Addresses                               property  aau       1 3 419539136 24 16885952                emits-change
.DnsOptions                              property  as        0                                        emits-change
.DnsPriority                             property  i         100                                      emits-change
.Domains                                 property  as        1 "home"                                 emits-change
.Gateway                                 property  s         "192.168.1.1"                            emits-change
.NameserverData                          property  aa{sv}    1 1 "address" s "192.168.1.1"            emits-change
.Nameservers                             property  au        1 16885952                               emits-change
.RouteData                               property  aa{sv}    3 3 "dest" s "192.168.1.0" "prefix" u 2… emits-change
.Routes                                  property  aau       2 4 108736 24 0 100 4 65193 16 0 1000    emits-change
.Searches                                property  as        0                                        emits-change
.WinsServerData                          property  as        0                                        emits-change
.WinsServers                             property  au        0                                        emits-change
```

#### Get sample property:

`
busctl get-property org.freedesktop.NetworkManager /org/freedesktop/NetworkManager/IP4Config/2 org.freedesktop.NetworkManager.IP4Config Addresses
`

```
aau 1 3 419539136 24 16885952
```

