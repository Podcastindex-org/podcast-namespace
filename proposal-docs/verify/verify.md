# The "podcast:verify" Specification

<small>Version 1.0 by [@pofmagicfingers](https://github.com/pofmagicfingers) - 2022.08.03</small>

<br>

## Purpose

"Claiming" a podcast in a podcast directory means that your customer can be authorized as the owner of that podcast in 
that directory. You might claim a podcast in Podchaser to add host information, in GoodPods to be notified of comments,
in Podcasterwallet to add value4value info, or in Spotify to add it to the Spotify dashboard for analytics.

A "quick-claim" is a programmatic way to claim a podcast, by taking your customer from a podcast directory to an 
authenticated page on their podcast host.

# For podcast directories
Run a podcast directory, and want to check that someone owns this podcast?

Benefits of using "quick-claim" are that it's one-click within a browser or webview, rather than sending a potential user an email. Assuming they are logged-in to their podcast host (or can log in), they can approve your request instantly and come back to your website within seconds.

Additional bnefits are:
* Anyone authorised to access the podcast host dashboard can claim the show (subject the user having the right permissions)
* Email confirmations are often subject to delay, fall into spam, or are blocked altogether
* Email confirmation emails can be faked and lead to security concerns
* The listing of an email address in an RSS feed leads to privacy issues and spam
* Email confirmations often lead to around [25%](https://www.quora.com/What-is-a-typical-abandonment-rate-for-email-verifications) abandomnent

## Check that quick-claiming is enabled in the RSS feed.
```xml
<podcast:lock
locked="yes" 
owner="some@email.com" 
auth="https://hostingprovider.com/claiming/" 
pub="MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEEVs/o5+uQbTjL3chynL4wXgUg2R9q9UU8I5mEovUf86QZ7kOBIjJwqnzD1omageEHWwHdBO6B+dFabmdT9POxg=="
/>
```

As above, you should see an `auth` field within the `<podcast:lock>` tag, this means the feed support quick claiming.
The pub attribute is a public key you will use later to check the autenticity of the hosting provider response.

## Give them a "claim this with your podcast host" button

You will redirect your user to the auth url present in the podcaster's RSS feed, with 1 to 3 URL parameters :

### `consumer`
Your directory/service base URL of the return_path.

It's recommanded to support open graph allowing the hosting provider to present a clean name and icon for your 
service while asking permission. If open graph is not supported, hosting provider will most likely fallback to 
hostname, path, favicon.

Splitting the return url into a `consumer` and a `return_path` gives us control of which part of the url is used 
for open graph data while ensuring the return path is tied to the hostname and identity presented to the user.

#### Examples:

`consumer=https://podcastindex.org`
```
PodcastIndex.org (podcastindex.org) 
The Podcast Index is here to preserve, protect and extend the open, independent podcasting ecosystem.

This service would like to verify you are the owner of this podcast. Do you want us to confirm ?
```

`consumer=https://podcastindex.org/quick_claim`
```
🗼PodcastIndex Quick Claiming (podcastindex.org) 
Claiming your podcast into The Podcast Index directory gives you cool stuff !

This service would like to verify you are the owner of this podcast. Do you want us to confirm ?
```

### `return_path`
A path relative to consumer parameter, to redirect with the result of the authentication.
If the consumer is enough, you can omit it.

#### Examples:

`consumer=https://podcastindex.org/quick_claim` (no return path)
➡️ full return url will be `https://podcastindex.org/quick_claim?token=[token]`

`consumer=https://podcastindex.org/quick_claim&return_path=/claimed.php`
➡️ full return url will be `https://podcastindex.org/quick_claim/claimed.php?token=[token]`

`consumer=https://podcastindex.org/quick_claim&return_path=../claim.php`
➡️ full return url will be `https://podcastindex.org/claim.php?token=[token]`

#### `guid`
If a `podcast:guid` is present in the RSS feed it SHOULD be included. If it's not we omit it or send an empty string.

We keep it simple, we should trust the users of our spec (in this case the hosting providers) : if they don't include a `podcast:guid`, I don't see the point to calculate it and give it to them, as they most likely don't have it on their hand and probably don't use it. If they would, it would be included in the feed.

If we don't have any podcast:guid, maybe the hosting provider has only partial support of the podcasting 2.0 spec, maybe they only support claiming and include the guid in the auth url, maybe it's a wordpress extension and there is only one podcast and no need to "select a podcast" when you're logged in. We can't know and guess all usages.

If we really want them to have a `podcast:guid` to use quick claiming, why not adding quick claiming _into_ `podcast:guid` ?

```xml
<podcast:guid 
auth="https://hostingprovider.com/claiming/"
pub="MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEEVs/o5+uQbTjL3chynL4wXgUg2R9q9UU8I5mEovUf86QZ7kOBIjJwqnzD1omageEHWwHdBO6B+dFabmdT9POxg==">
    [here the podcast guid]
</podcast:guid>
```

It kind of fits, as it's the feed identity control.

### PHP Example

Here's an example in PHP of your button:

 ```php
<a 
class="btn btn-default"
href="<?php echo $feed_auth_url."?consumer=https://myservice.com/quick_claim&guid=".$feed_guid; ?>"
>
Claim this now
</a>
 ```

## Handle the response

If the user wants to claim the podcast, they will confirm it inside their hosting service. You will get a GET request to the return URL computed with the consumer and return_path.

The GET request will contain only one parameter : `token`.

This parameter will be a JSON Web Token including the following data :

```typescript
type QuickClaimResponse = {
	guid?: GUID;
	accepted: true;
} | {
	guid?: GUID;
	accepted: false;
	failureReason: "back" | string;
};
```

Using JWT lets us ensure the hosting provider wrote the response. It also allow us to define or not an expiration 
date and such. ([More info](https://jwt.io/))

The JWT must use an asymetric signing algorithm (ES256 for example). The response is signed by the hosting provider 
private key, and it can be verified using the `pub` attribute of the tag (put there by the same hosting provider).

You can verify the signature in PHP with a JWT library, if it fails to decode, it means the signature is wrong or the 
token has expired:
```
$result = JWT::decode($quickClaimResponse, $feed["podcast:lock"]["pub"]);
```

As a podcast directory/service you have now the confirmation the user was indeed authenticated, confirmed the 
operation, and that its the same entity giving you this information and producing the RSS feed.

# For podcast hosts
Quick-claim is designed to allow your customers to demonstrate that they own their podcast on third-party services, 
like Podchaser or Goodpods, as an example.

The benefits of using "quick-claim" are that it's one-click for your customers from the service they wish to claim, 
direct to you. You can monitor the services your users are using, and can give multiple people access to claiming a 
podcast on a separate service based on your service's access levels.

The email in your RSS feed need not be the registration email of the user on this third-party service, thus lowering 
your customer support calls and streamlining access for your customers. Ease of use on third-party services retains 
the customer with your company, thus lowering churn, and possibly giving them more access and interaction, prolonging 
their activity with you.

If you're a podcast host wanting to add quick claiming for your customers, then here's how you can do it painlessly:

## Add a `podcast:lock` tag in your RSS feeds

The tag needs two attributes :

a. `auth` - a URL to a page that is protected on your server by customer login. It is highly recommended that this 
            be an `https` address.
b. `pub` - the public key corresponding to the private key you'll use to sign claiming requests.

Example: `<podcast:lock auth="https://amazingpodcasthost.example.com/claiming" pub="MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEEVs/o5+uQbTjL3chynL4wXgUg2R9
q9UU8I5mEovUf86QZ7kOBIjJwqnzD1omageEHWwHdBO6B+dFabmdT9POxg==" />`


## Host a page for a user to agree to "claim" a podcast

Your claiming auth url will be called with 1 to 3 parameters :

#### `consumer`
The directory/service base URL.

This url must be used to present the permission asker to the user. It's recommanded for services to support 
open graph on this URL allowing the hosting provider to present a clean name and icon. If open graph is not supported,
you will most likely fallback to title and favicon, hostname only, or full url.

Splitting the return url into a `consumer` and a `return_path` gives us control of which part of the url is used for 
open graph data while ensuring the return path is tied to the hostname and identity presented to the user.

#### `return_path`
A path relative to consumer parameter, to redirect with the result of the authentication.
If the consumer is enough, you can omit it.

Examples:

`consumer=https://podcastindex.org/quick_claim` (no return path)
➡️ full return url will be `https://podcastindex.org/quick_claim?token=[token]`

`consumer=https://podcastindex.org/quick_claim&return_path=/claimed.php`
➡️ full return url will be `https://podcastindex.org/quick_claim/claimed.php?token=[token]`

`consumer=https://podcastindex.org/quick_claim&return_path=../claim.php`
➡️ full return url will be `https://podcastindex.org/claim.php?token=[token]`

#### `guid`
If a `podcast:guid` was present in the RSS feed.

### Here's an example in PHP:

 ```php
<?php
$HOSTING_PRIVATE_KEY = "MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgevZzL1gdAFr88hb2
OF/2NxApJCzGCEDdfSp6VQO30hyhRANCAAQRWz+jn65BtOMvdyHKcvjBeBSDZH2r
1RTwjmYSi9R/zpBnuQ4EiMnCqfMPWiZqB4QdbAd0E7oH50VpuZ1P087G";

if (!$user['loggedin']) {
    // this user is not logged in. Take them to log in
    // Retain the consumer, return_path, guid, and bring them back here
	//
	exit;
}

// You might want to give confirmation that the podcast they are claiming
// is the correct podcast.
// Grab the podcast details in an array from your local systems.
// Check that the podcast is owned by this person, of course.

$consumer = $params["consumer"];
$return_path = $params["return_path"];
$podcast = lookup_from_guid($params['guid']);

// If podcast is not found on this user we redirect with an error message:

if(!$podcast) {
	  header(
		    "Location: ".
		    $consumer.
		    $return_path.
		    "?token=".
		    build_jwt_token(
			      [
				      "accepted" => false,
				      "error" => "Podcast could not be found for this user"
			      ], 
			      $HOSTING_PRIVATE_KEY
		    )
	  );
	  exit();
}

$guid = $podcast["guid"];


// If we already shown the page and got user response 
// 
// checking csrf depends on your context, but is a strong recommendation for security
if(check_csrf()) exit("Bad request");

$action = $params["action"];
if($action == "accept") {
	header(
		"Location: ".
		$consumer.
		$return_path.
		"?token=".
		build_jwt_token(
			[
				"accepted" => true
			], 
			$HOSTING_PRIVATE_KEY
		)
	);
	exit();
} else if ($action == "back") {
	header(
		"Location: ".
		$consumer.
		$return_path.
		"?token=".
		build_jwt_token(
			[
				"accepted" => false,
				"error" => "back"
			], 
			$HOSTING_PRIVATE_KEY
		)
	);
	exit();
}

$service = opengraph($consumer); 

$claim_fields = <<<TAGS
<input type="hidden" name="guid" value="$guid" />
<input type="hidden" name="consumer" value="$consumer" />
<input type="hidden" name="return_path" value="$return_path />
TAGS;

?>
<h1>Claiming your podcast</h1>
<img src="<?= $service["icon"] ?>">
<h3>
	<?= $service["name"] ?>
	<small>(<?= parse_url($consumer,PHP_URL_HOST) ?>)</small>
</h3>
<p>This service wants us to confirm you have control over this podcast : <?= $podcast["name"]; ?></p>
<form method="post" action="/quick_claim">
<?= $claim_fields ?>
<?= put_csrf_protection() ?>
<input type="hidden" name="action" value="accept" />
<input type="submit" value="I do">
</form>
<form method="post" action="/quick_claim">
<?= $claim_fields ?>
<?= some_csrf_protection ?>
<input type="hidden" name="action" value="back" />
<input type="submit" value="cancel this request">
</form>
 ```

The above will successfully check that your user is authenticated and send back the token to the directory service 
if the user agrees.

## Full example workflow

This section will summarize everything into one big example.

Say **DIRECTORY** is a podcast directory, **HOST** is a podcast hosting service, **PODCAST** is a show 
and **CREATOR** is a member of this show, allowed to claim the **PODCAST** on directories.

**HOST** adds this to the RSS feed:

```xml
<podcast:guid>ead4c236-bf58-58c6-a2c6-a6b28d128cb6</podcast:guid>
<podcast:lock auth="https://host.com/studio/quick_claim/" pub="MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEEVs/o5+uQbTjL3chynL4wXgUg2R9
q9UU8I5mEovUf86QZ7kOBIjJwqnzD1omageEHWwHdBO6B+dFabmdT9POxg==" />
```

**DIRECTORY** presents a button "Quick claim".

When **CREATOR** clicks on it, they are redirected to **HOST** with this URL :
`https://host.com/studio/quick_claim/?guid=ead4c236-bf58-58c6-a2c6-a6b28d128cb6&consumer=https://directory.com/quick_claiming/ead4c236-bf58-58c6-a2c6-a6b28d128cb6&return_path=/return`

**HOST** can use consumer parameter to fetch some data (name, icon, description) through open graph. In this example 
DIRECTORY use a consumer URL including podcast guid and could use it to add some podcast info into the open graph 
description.

**HOST** asks for **CREATOR** confirmation in a logged only area after verifying on their own responsability if user 
has indeed access to this podcast.

When **CREATOR** accept the claim request, the **HOST** will generate a token, signed by their private key, 
authenticating the CREATOR decision.

Unsigned :
```json
{
  "guid": "ead4c236-bf58-58c6-a2c6-a6b28d128cb6",
  "accepted": true
}
```

Signed : `eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJndWlkIjoiZWFkNGMyMzYtYmY1OC01OGM2LWEyYzYtYTZiMjhkMTI4Y2I2IiwiYWNjZXB0ZWQiOnRydWV9.eOXYFi9uUSUAKWcI8GdJ15RIhjoCvR0l9TUCPsqhsTYqaGFTwbH6zXzYqIqhxmtSotvL8ZLumP64LRFBjHX5Mw`

<details>
<summary>Note : You can follow along and "translate" online</summary>

Decode/Encode online with : https://jwt.io/

Private Key
```
-----BEGIN PRIVATE KEY-----
MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgevZzL1gdAFr88hb2
OF/2NxApJCzGCEDdfSp6VQO30hyhRANCAAQRWz+jn65BtOMvdyHKcvjBeBSDZH2r
1RTwjmYSi9R/zpBnuQ4EiMnCqfMPWiZqB4QdbAd0E7oH50VpuZ1P087G
-----END PRIVATE KEY-----
```

Public Key
```
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEEVs/o5+uQbTjL3chynL4wXgUg2R9
q9UU8I5mEovUf86QZ7kOBIjJwqnzD1omageEHWwHdBO6B+dFabmdT9POxg==
-----END PUBLIC KEY-----
```

</details>

**CREATOR** are redirected to the **DIRECTORY** with this URL:

`
https://directory.com/quick_claiming/ead4c236-bf58-58c6-a2c6-a6b28d128cb6/return?token=eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJndWlkIjoiZWFkNGMyMzYtYmY1OC01OGM2LWEyYzYtYTZiMjhkMTI4Y2I2IiwiYWNjZXB0ZWQiOnRydWV9.eOXYFi9uUSUAKWcI8GdJ15RIhjoCvR0l9TUCPsqhsTYqaGFTwbH6zXzYqIqhxmtSotvL8ZLumP64LRFBjHX5Mw`

**DIRECTORY** can now check the token parameter to ensure it has been correctly signed with the private key 
corresponding to the public key seen in the RSS feed, and in this case, the claiming request has been accepted.

Other responses could be, for example :

`
https://directory.com/quick_claiming/ead4c236-bf58-58c6-a2c6-a6b28d128cb6/return?token=eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJndWlkIjoiZWFkNGMyMzYtYmY1OC01OGM2LWEyYzYtYTZiMjhkMTI4Y2I2IiwiYWNjZXB0ZWQiOmZhbHNlLCJlcnJvciI6IlVzZXIgY2FuJ3QgYWNjZXNzIHRvIHRoaXMgc2hvdyJ9.MDkZanxlukjQRAj5zd2GoWetAwMWPZs1RU24HdSw8LJm3Z73kL2U4gHMOJUg62LtZdIoH3tktSR0w-1Ltuo4Ig
`

```json
{
  "guid": "ead4c236-bf58-58c6-a2c6-a6b28d128cb6",
  "accepted": false,
  "error": "User can't access to this show"
}
```

`
https://directory.com/quick_claiming/ead4c236-bf58-58c6-a2c6-a6b28d128cb6/return?token=eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJndWlkIjoiZWFkNGMyMzYtYmY1OC01OGM2LWEyYzYtYTZiMjhkMTI4Y2I2IiwiYWNjZXB0ZWQiOmZhbHNlLCJlcnJvciI6ImJhY2sifQ.TP8h8Hwh7oRpcuTPXOeqrO46sNwlwC4RLdyMtdFqZQfsS0pUT71_ljoUWq3a0o_hUjuVvPoWnDXar7o2BbLw6w
`

```json
{
  "guid": "ead4c236-bf58-58c6-a2c6-a6b28d128cb6",
  "accepted": false,
  "error": "back"
}
```

Here are my thoughs on this idea and how to implement it, feel free to make any remarks about it.

JWT seems to me to be the middle ground between complexity and simplicity for a decentalized authorization system.
It only requires the hosting provider to add a private key somewhere and print the pub key on the feed. 
Signing/Verifying JWT is quick and easy and there are libraries in almost any languages. It basically falls down 
to something like `[verify|sign]JWT(content, priv/pub_key)` in most languages.

Open Graph is a nice bonus "out of the box" for permission asker identity.

We could also make this a kind of API spec, and add more data to the token.
Maybe mimick oauth and add a scope param asking specifically for some data, permissions about the 
feed. (`scope="stats,edit,delete,admin"` etc)

That's a big subject and a spec of its own. That could be a nice addition but it has to be done in a way we can trust 
the system and all information it conveys.
