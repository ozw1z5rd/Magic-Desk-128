# Magic Desk 128 (1MB)

<img src="rev1\images\render-top-names.png" alt="Render top" width="400"/><br/>
This is a Commodore 128 version of the popular Commodore 64 Magic Desk cartridge.
The Magic Desk cartridge is a simple bank switching ROM cartridge that uses the first address of I/O1 ($de00) to select ROM banks. The difference between this C128 Magic Desk format and the C64 version is that the C128 version has 16kB ROM per bank instead of 8kB and therefore supports twice as much ROM. Theoretically it would support up to 4MB (256x16kB).  
This is the first version of the C128 Magic Desk cartridge and it has room for up to 1MB of ROM divided into 64 banks of 16kB.

The cartridge was designed with SST39 flash chips in mind but other 32-pin flash or EPROM/EEPROM chips can be used too as long as they are pin-compatible.

### Startup
After power-up or reset, ROM bank 0 (the first 16kB of the LOW chip) is active and accessible at both $8000-$bfff and $c000-$ffff.  
That first bank needs to contain the regular cartridge initiation bytes such as CBM magic word, autostart configuration and start vectors if you want your cartridge to autostart.

### Switching banks
Write an address of a ROM bank such as #00-#63 (#$00-#$3f in hex) to $de00 to switch bank. Set the value to 63 to switch to the last bank of the second ROM and value 0 to switch back to the first bank in first ROM or any bank in between.
The size and amount of ROM chips is flexible. It's even possible to mix and match chips of different sizes to match your needs but you need to be aware of the available bank numbers when doing so.
The LOW chip contains banks 0-31 and the HIGH chip contains 32-63. 
It doesn't make sense to add a HIGH chip if there's anything less than a 4Mbit/512kB (SST39SF040) chip in the LOW position but it is possible. You just have to skip some of the banks from the LOW range and skip to bank 32 to read from the HIGH chip.

## ROM population examples
|LOW chip   |HIGH chip	|Banks      |Total size |
|----------	|----------	|-----	    |----------	|
|SST39SF010	|           |0-7        |128kB      |
|SST39SF010	|SST39SF010	|0-7, 32-39 |256kB		|
|SST39SF020	|           |0-15		|256kB	  	|
|SST39SF040	|           |0 -31      |512kB      |
|SST39SF040	|SST39SF020	|0-47		|768kB		|
|SST39SF040	|SST39SF040	|0-63		|1024kB	  	|

If you use a smaller chip such as the SST39SF010, the data in the eight available banks will be repated at bank address 8-15, 16-23, 24-31.

## BOM
|identifier |Part                 |Comments                         |
|----------	|----------	          |-----							|
|U1         |74LS273              |or 74HCT273/74AHCT273            |
|U1         |74LS02               |or 74HCT02/74AHCT02              |
|U3         |32-pin flash/EPROM   |First ROM. SST39SF or compatible |
|U4         |32-pin flash/EPROM   |Second ROM (optional)            |
|C1,C2,C3,C4|100nF ceramic        |5mm pitch                        |

## Software
### Magic Cartridge Generator
Zzarko has made a Python script and a menu program that can be used to generate menu-driven multi-cart images that can be put on this cartridge.  
Create a cartridge with all your favorite C128 utils or games. The requirement is that they consist of a single prg-file (no multi-load programs). 
Zzarko's Magic Cartridge Generator along with documentation and examples can be found at https://bitbucket.org/zzarko/magic-cartridge-generator
