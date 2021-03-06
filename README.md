# r509-validity-redis [![Build Status](https://secure.travis-ci.org/r509/r509-validity-redis.png)](http://travis-ci.org/r509/r509-validity-redis) [![Coverage Status](https://coveralls.io/repos/r509/r509-validity-redis/badge.png?branch=master)](https://coveralls.io/r/r509/r509-validity-redis?branch=master)

This project is related to [r509](http://github.com/r509/r509), [r509-ca-http](http://github.com/r509/r509-ca-http), and [r509-ocsp-responder](http://github.com/r509/r509-ocsp-responder), allowing certificate validity and revocation information to be read and written to a Redis backend.

When a certificate is issued, we want this sent to Redis:

    HMSET "cert:<issuer>:<serial>" status 0

When revoked:

    HMSET "cert:<issuer>:<serial>" status 1 revocation_time <timestamp> revocation_reason 0


To get the status of a certificate:

    HGETALL "cert:<issuer>:<serial>"

The "status" field can be one of:

    R509::Validity::VALID
    R509::Validity::REVOKED

The "revocation\_reason" field can be one of:

    null
    0  (unspecified)
    1  (keyCompromise)
    2  (cACompromise)
    3  (affiliationChanged)
    4  (superseded)
    5  (cessationOfOperation)
    6  (certificateHold)
    8  (removeFromCRL)
    9  (privilegeWithdrawn)
    10 (aACompromise)
