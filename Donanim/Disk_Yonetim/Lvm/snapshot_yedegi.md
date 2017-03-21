#### LVM Snapshotting

* we can back up that volume without having to worry about data being changed
  while the backup is going on, 
* and we don't have to take the database volume offline while the backup is
  taking place.
* Snapshots create an image of an entire disk image
* kullanisli gelmedi, erteliyorum.
   


Reading from the original volume
The user reads from vg0-lv0. The request is forwarded and the user retrieves data from vg0-lv0-real.

Writing to the original volume The user writes to vg0-lv0. The original data is
first copied from vg0-lv0-real to vg0-snap1-cow (This is why it’s called COW,
Copy On Write). Then the new data is written to vg0-lv0-real. If the user
writes again to vg0-lv0, modifying data that has already been copied to
vg0-snap1-cow, the data from vg0-lv0-real is simply overwritten. The data on
vg0-snap1-cow remains the data that was valid at the time of the creation of
the snapshot.

