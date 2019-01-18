# AMPscript - Update Profile Attributes

Use AMPscript in a landing page in order to unsubscribe a contact, or update its [DoNotTrack](https://help.salesforce.com/articleView?id=mc_es_do_not_track_email_opens_and_clicks.htm&type=5) feature or simply a normal profile attribute.

## Update Unsubscription

If you pass some parameters in the URL you can interact within a Cloud Page with AMPscript. 

```vbscript
%%[
set @subkey = requestparameter("subkey")
set @email = requestparameter("email")

set @Subscriber = CreateObject("Subscriber")
     SetObjectProperty(@Subscriber, "SubscriberKey",@subkey)
     SetObjectProperty(@Subscriber, "EmailAddress",@email)
     SetObjectProperty(@Subscriber, "Status", "Unsubscribed")
set @Status = InvokeUpdate( @Subscriber, @createErrDesc, @createErrNo, @createOpts)
]%%
```

In this snipet we use [APIs functions with AMPscript](https://developer.salesforce.com/docs/atlas.en-us.noversion.mc-programmatic-content.meta/mc-programmatic-content/ampscriptSOAPAPI.htm) to update the subscribers `status` attribute

> **Note:** It is recommended to encrypt the URL parameters to add more security. 





## Update DoNotTrack feature

In this example we will update the `DoNotTrack` attribute and change its value from `false` to `true` 

```vbscript
%%[
set @subkey = requestparameter("subkey")
set @email = requestparameter("Email")

set @Subscriber = CreateObject("Subscriber")
    SetObjectProperty(@Subscriber, "SubscriberKey",@subkey)
    SetObjectProperty(@Subscriber, "EmailAddress",@email)
    /* Create the Attribute */
    Set @att = CreateObject( "Attribute" )
    SetObjectProperty( @att, "Name", "DoNotTrack")
    SetObjectProperty( @att, "Value", "true")

	/* Set the attribute to the subscriber */
	AddObjectArrayItem(@subscriber, "Attributes", @att)
    SetObjectProperty( @subscriber, "DoNotTrack", "true" )
/* Performing the update */
set @Status = InvokeUpdate(@Subscriber, @createErrDesc, @createErrNo, @createOpts)
]%%
```

> **Note:** if you can create it following [this documentation](https://help.salesforce.com/articleView?id=mc_es_configure_the_preference_attribute_for_do_not_track_email_opens_and_clicks.htm&type=5)





## Update normal attribute

In this example we are just updating an custom attribute called `source`

```vbscript
%%[ 
set @Subscriber = CreateObject("Subscriber")
SetObjectProperty(@Subscriber,"SubscriberKey","marketing@thibs.com")
SetObjectProperty(@Subscriber,"EmailAddress","marketing@thibs.com")

/* Create the Attribute */
Set @att = CreateObject( "Attribute" )
SetObjectProperty( @att, "Name", "source")
SetObjectProperty( @att, "Value", "email")

/* Set the attribute to the subscriber */
AddObjectArrayItem(@subscriber, "Attributes", @att)

set @Status = InvokeCreate( @Subscriber, @createErrDesc, @createErrNo, @createOpts)
]%%
```



