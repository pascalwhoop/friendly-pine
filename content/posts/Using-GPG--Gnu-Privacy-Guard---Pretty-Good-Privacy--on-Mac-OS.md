---
title: Using GPG (Gnu Privacy Guard / Pretty Good Privacy) on Mac OS
date: '2017-07-02T13:47:29.278Z'
subtitle: Installation
excerpt: Installation
layout: post
---
Using [brew](https://brew.sh/) we can quickly install it

`$ brew install gpg`

#### Get your key and share it

now we can generate a key pair

`$ gpg --generate-key`

Enter your name, email and a Passphrase and the tool generates the data for you.

Now you can export your public key and share it with others, either directly, through a website or via a keyserver. I would recommend [pgp.mit.edu](https://pgp.mit.edu/) or if you like a fancy UI a bit more, try [keybase.io](http://keybase.io).

`$ gpg --export -a > ~/Desktop/pascal.public.gpg.asc`

My file content looks as such:

    -----BEGIN PGP PUBLIC KEY BLOCK-----
    mQENBFlY4MkBCADbIFsb4mZ9FyrUsoNfOqmAPMS+sIlBczF4Zmw9LKO/coq9+6N9  
    3yE5P8BvS/EWfP9zwTyfS0Tjv889uqG/oM/RPO3fAJewNR6vWWcrufpvyBApqYIA  
    8ph+RvKqLh+qNkoTu+n1bF2zELJaKSWVhaaLEbW0+ijvm6mdn+NNukkK50fU4MuP  
    7z6zWBszldc+h0Rfwo8HdR1oCZidJHSaqDjKmpwdvLTB24GIgSmoUWPYNEjIW7NT  
    YAIPHW29UaJoAJlsW4MsDVVMqu4BV/OsZkm+6OW9t/EGH3qWs/gLCztnlVyJSu0t  
    clgIEIsD/jZj4cnUX6cnW+eqn6jzkMYIm6QxABEBAAG0LVBhc2NhbCBCcm9rbWVp  
    ZXIgPHBhc2NhbC5icm9rbWVpZXJAZ21haWwuY29tPokBVAQTAQgAPhYhBLtuUG1j  
    wlgpeiraSBxJon1jqLWgBQJZWODJAhsDBQkDwmcABQsJCAcCBhUICQoLAgQWAgMB  
    Ah4BAheAAAoJEBxJon1jqLWg0dQH/iGdv1cbFVx3Z7gPVPkMisAaMRX34Vn0JV8j  
    v5BNE9/AEZhmujxmKaYCTczyZdiN4VWBk821nfvnT5FTJ8Glnwhhqft91C3hB7RF  
    aAYDAsINT0quaY7F9qG0mI2EGm1fFlQV7Pko2aiMYiQ8bw53rq3k6UanPLZ/zMJl  
    fVruuSImxd9KLUcCobyp5FxeRGasxSvTCshAwKVTaXV7azT/M+5SOPWkRqCG6fcL  
    89Veq/8+4FHB4D7KbxcE8x+nwS7x/Qf1uHxaMyjMijD5S75TAoLqIyHOeHi4kPhw  
    tr11EeuDPPppCg3VTX77BRUypEXiGUUmOJzHtFVc/Z/DZmsavnC5AQ0EWVjgyQEI  
    AMszJfruFQxl+RQSbK8n8FOE+8dt+6YWTu/T5lUURZxbW/So100eK/+v1nf/QDvU  
    di9l6WZOuyQR/n6VsF15G5QTK34vtg9FuuQgXOn0ydKcK3WH08glFOMApFcM5IZA  
    Dm6C5bMxOJI7Kng4OZYQmydyGkZ08ZuCCWWOeHcQgOVGXh0QMjV7sJ0wGbkwPgTH  
    4YYH97TZ4HiWt6EM2r5o1j6pjoAMVRs6ibfaZCZUS1uip+b9wZc+HMtyqO8sFSaq  
    QtMaKXAijJ36VRqnn+j4FScOauxTcNBNWcO5KyeyJSZO/g6oMhdHHscV9UJi8PgS  
    ALTpML+ankJ+GZpDz+S3KzkAEQEAAYkBPAQYAQgAJhYhBLtuUG1jwlgpeiraSBxJ  
    on1jqLWgBQJZWODJAhsMBQkDwmcAAAoJEBxJon1jqLWgfpgH/2GqmAgBtlnp7GVv  
    qE1xc1/Zpu7CgpvXhUxhTPq0yQ6GidFNcZqDSNiT7oecH2XIKPKHW754CtYfNK/M  
    r0p/LbKw8oHHDpXVwl9CPTibjL+q4rh0qdl6h57JoCeKV52penShTUp5mEZHIt+7  
    x4+Gx0tUv/OQKCJCblD4sb+sQgPVVZIojSENLmVctu56QwPl97aLFZoT0ddQ+y60  
    7aOgTJxFg48Me8XVMkPCZxCENZF2o6i8Ud+05rki6XaH760hUVhgfGrjnuggMhFO  
    0ZL0JkFCwdZ0gidxOCcWvmtAlF3pdjMnxlhHU42tQO2gTxnCLCIntcXI1FL6UCQ2  
    lrOOOHg=  
    =XCx0  
    -----END PGP PUBLIC KEY BLOCK-----

Upload the content of the file to a server of your choice.

#### Verify

To get your keys fingerprint, type

`gpg --fingerprint`

mine is this

    BB6E 506D 63C2 5829 7A2A  DA48 1C49 A27D 63A8 B5A0

try encrypting a message with your own public key and decrypting it again

    $ echo "this is a secret message" | gpg -ea -r pascal.brokmeier@gmail.com > message_crypted.pgp  
    $ gpg --decrypt message_crypted.pgp

If that works, you are ready to send and receive messages with PGP/GPG.
