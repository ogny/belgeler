#### intel raid controller kartlarda yeni disk eklemek icin
* unconfigured good durumdaysa;
  - controller sag klik "create new virtual drive"
  - advanced'ta diski grup altina tasiyip create virtual group
  - ozelliklerde 'write ahead' yerine 'write through' sec, bitir.

[E0:S0,E1,S1,…] Specifies when one or more physical devices need(s) to be
specified in the command line. Each [E:S] pair specifies one
physical device where E means device ID of the enclosure in
which a PD resides, and S means the slot number of the
enclosure.
In the case of a physical device directly connected to the SAS
port on the controller, with no enclosure involved, the format of
[:S] can be used where S means the port number on the
controller. For devices attached through the backplane, the
firmware provides an enclosure device ID and MegaCLI expects
the user input in the format of [E:S]. In the following sections,
only the format, [E:S], is used in the command descriptions,
although both formats are valid.

only the format, [E:S], is used in the command descriptions,
