:warning: I am not a programmer, I am a nerd who needed to solve a problem.
Expect the code quality to be appropriately shocking.
Caveat emptor.

# Ripcap

Split matching frames from a pcap file

 ## Options
 
|                         |                                                  |
|-------------------------|--------------------------------------------------|
| -h, --help              | Show help message and exit                       |
| -e ETHER, --ether ETHER | Match MAC Address                                |
| -i INET, --inet INET    | Match IP Address                                 |
| -p PORT, --port PORT    | Match Port                                       |
| -P {icmp,tcp,udp}, --proto {icmp,tcp,udp} | Match Protocol                 |
| -f TIME, --from TIME    | Start Time                                       |
| -t TO, --to TO          | End Time                                         |
| -c COUNT, --count COUNT | Maximimum number of matches                      |
| -x HEX, --hex HEX       | Match byte string                                |
| -v, --verbose           | Additional output (gives a count of packets)     |

## Hex

Accepts ascii with or without spaces, no other options.  eg:

* `--hex "c0 ff ee"`
* `--hex c0ffee`

## Times

Examples of suitable dates:

* 2021-06-10
* June 10
* June 10, 2021

Examples of suitable times:

* 13:00
* 13:00:00
* 01:00pm
* 1pm

Dates and times can be combined in most sensible combinations, eg `--from "June 10, 1pm" --to "2021-06-11 13:00"`

* If a time is not provided, it's assumed to mean midnight
* If a date is not provided:
  * The `from` date is assumed to be the date of the first packet in the source capture.
  * The `to` date is assumed to be the same date as the `from` date (or the first packet in the capture if a `from` date wasn't provided)

eg:

* `--from 14:00 --to 15:00` will capture from 14:00 to 15:00 on the date of the first packet in the capture.
* `--from "2021-06-13 14:00" --to 15:00` will capture from 14:00 to 15:00 on June 13th.

Beware:

 * Because unspecificed times evaulate as midnight, `--from "June 10" --to "June 10"` will match nothing as to & from both evaluate to the same time
 * If you don't specify a date, and your capture starts after the specified time, it will not attempt to resolve this.
   * So if your capture starts at 10pm and you ask `--from 13:00 --to 14:00`, you won't match tomorrow, you'll match nothing.