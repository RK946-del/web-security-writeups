VULNERABILITY:
Access to account data is controlled via a user-supplied 'id' parameter without verifying object ownership, resulting in an IDOR vulnerability.

DEVELOPER ASSUMPTION:
user supplied 'id' wont be tampered and object ownership check is unneccessary.

WHY THIS IS DANGEROUS:
attacker can intercept the request and change user control 'id' parameter to retrive other accounts data.

EXPLOITATION STEPS:
1) Log in as a normal user(to become authenticated).visit my-account path intercept it.
2) change id=yourid to id=administrator 
3) response:
    200 ok-> logged in


postswigger lab: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure