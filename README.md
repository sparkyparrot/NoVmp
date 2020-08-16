<p align="center">

  <a href="https://www.vtil.org/">
    <img width="256" heigth="256" src="https://i.imgur.com/5yt7EsH.png">
  </a>  

  <h1 align="center">NoVmp</h1>
</p>

### VMProtect? Nope.
NoVmp is a project devirtualizing VMProtect x64 3.0 - 3.6 (latest) into optimized VTIL and optionally recompiling back to x64 using the [Virtual-machine Translation Intermediate Language](https://github.com/vtil-project/VTIL-Core) library. It is rather experimental and is mostly a PoC I wanted to release. Most things can be improved especially with the new NativeLifters repo, but it did not exist back in the time this was written.

# Usage
NoVmp  accepts **unpacked binaries**, so if your binary is packed you'll have to dump it first, additionally if you did dump it using a tool like Scylla, you'll have to provide the original image base using the `-base` parameter like so:

`-base 0x14000000` 

By default NoVmp will parse every single jump into a VM, if you are only interested in a number of **specific** virtualized routines you can use the `-vms` parameter like so with relative virtual addresses:

`-vms 0x729B81 0x72521`

These addresses should be pointing at the VMEnter, as shown below:
![VMEnter](https://i.imgur.com/oIrgvVh.png)

By default section discovery is automatic, but in case your calls are not being changed you should try adding the VMProtect section name into the section list like:

`-sections .be0`

The `.<vmp>1` section is the merged VMProtect DLL which can be ignored.

If you wish to disable optimization you can invoke it with `-noopt` and to test the experimental x64 compiler you can use `-experimental:recompile`, experimental standing for borderline broken.

# Known bugs
- Jump tables are not supported.
- Binaries compiled with relocations stripped will require some manual code changes as it changes the way basic blocks function, this is left to the user and won't be fixed.
- Experimental compiler is a horribly designed demo.

# License
NoVmp is licensed under the GNU General Public License v3.
