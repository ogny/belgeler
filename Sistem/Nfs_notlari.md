> By using smb in stead of nfs my disk spins up less often. But it still
> spins up with no apparent reason (no computer in the network is on)
that's tipical of linux. Some measures can be taken to alliviate the problem, 
but they are not 100% effective.

nfs has another characteristic that might contribute to the problem: the 
server stores some status on disk, so that after a crash/porweroff/poweron  it 
can resume operations and continue serving the same clients, without they even 
noticed what happened (but the clients blocks if they try to contact the 
server -- when it comes back the client de-blocks and continues).

With this nfs philosofy in mind, Alt-F sets the save-status directory on disk.
This was a design decision, as I could setup those directories in memory, 
trying to avoid the disk spinup.  The problem is that on a server crash you 
would have to poweroff the clients also, as the server could not resume 
operations.
Unless... (this is becaming a class!) that in the client you mounted the nfs 
share in "soft" mode (not "hard" -- but then you could have data loss...


