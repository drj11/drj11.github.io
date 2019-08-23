[brutal]: #title "GDPR: Encrypt Your Login Page"
[brutal]: #author "David Jones"
[brutal]: #date "2019-08-23"

On Twitter I recently asserted that presenting an unencrypted
login page was a GDPR violation.
Without much evidence to back me up.

# General Data Protection Regulation

Does GDPR require that the login page be encrypted?

TL;DR: Yes (I think).

I'm not a legal anything, but we can all read the text of
[the GDPR](https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng)

Article 4 of the GDPR defines "personal data" to mean:
"any information relating to an identified or identifiable natural
person (...); an identifiable natural person is one
who can be identified, directly or indirectly, in particular by
reference to an identifier such as a name, an identification
number"

So I think that makes it clear that
your login id on a website is personal data.

Article 25 says the implementation of the data processing should
be designed and costed so that
it is designed to implement the data-protection principles, and
that this should take into account the state-of-the-art.

Article 25 looks like it is written to
pre-empt the "we ran out of money to implement TLS" excuse.
In this, hypothetical, case, TLS should have been budgeted for,
and because TLS is state-of-the-art it should be a designed-in measure.

Article 32 says that personal data shall be encrypted.
There is however, some allowance for a judgement call here.
One must take into account the state-of-the-art,
and the cost of implementation.

I personally feel that since encrypted login is
state-of-the-art across the (web) industry, and
that the direct cost is minimal to zero, then
that leaves little wiggle room.
Data should be encrypted.
You're login page should be encrypted.

## Article 5

This is all in the context of the GDPR's Article 5,
_Principles relating to processing of personal data_.
Clause 1(f) of which says that personal data shall be: 

"processed in a manner that ensures appropriate security of
the personal data, including protection against unauthorised or
unlawful processing and against accidental loss, destruction or damage,
using appropriate technical or organisational measures
(‘integrity and confidentiality’)."

To me this is the belt and the braces.

I read Article 5 as saying that regardless of what other articles
say specifically, your login details should be encrypted so that
they are protected "against unauthorised or unlawful processing".

Encrypt your login page (side-eyes rosaland.info).
