# Collect Subject Alternative Name entries from a server

This is a simple Python script that:

1. Makes a connection to a server (with SNI)
2. Requests its X509 certificate
3. Prints the Common Name and Subject Alternative Names within the server certificate

Note that many certificates include wildcards (e.g. `*.google.com`), which may
not be supported by tools if you pipe the output from this tool to another.
Use the argument `--trim-wildcards` to remove the `*.` and display only the
remainder of the domain/subdomain.


## Install

```bash
# Optional - create a virtual env
python3 -m venv env
source env/bin/activate

pip3 install -r requirements.txt
chmod +x get_sans
```

## Usage

    get_sans [-h] [-p PORT] [--trim-wildcards] domain

### Positional Arguments:

#### domain

Domain name to retrieve the certificate from. Can optionally attach a port number with a colon (google.com:443)

## Optional/Named Arguments:

#### -h, --help

show help message and exit

#### -p PORT, --port PORT

Optional port number. Defaults to 443.

#### --trim-wildcards

Trim the *. from the front of any wildcard DNS names. Should result in only
valid domains output from the tool (which can be piped to another tool)

## Examples

```bash
$ ./get_sans google.com
*.google.com
*.android.com
*.appengine.google.com
*.bdn.dev
*.cloud.google.com
*.crowdsource.google.com
...
```

```bash
$ ./get_sans google.com:443
*.google.com
*.android.com
...
```

```bash
$ ./get_sans -p 443 google.com
*.google.com
*.android.com
...
```

```bash
$ ./get_sans --trim-wildcards google.com
google.com
android.com
...
```