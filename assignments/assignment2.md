# Assignment 2
## Exercise 1
1) The two invariants of the state is the count in a request can never go negative and a purchase must correspond to an existing request. Both invariants are important, however it is more important that a purchase corresponds to an existing request than that the number of purchases is appropriate for the number of requests. Without this invariant the overall purpose of the concept would not be upheld. The action whose design is most impacted by these invariants is purchase. Purchase preserves these invariants by requiring that a request exists for the item trying to be purchased. The count invariant is preserved by insuring that the count of the purchase is equal or less that the count of the request, additionally the effect of purchase is to decrement count of the item by the count purchased.
2) If someone purchases an item from the registry, and then that item is removed from the registry, there would be a purchase not corresponding to a existing request. This can be fixed by adding a count variable to the removeItem action. This new addition would change the requires to be that the count must be equal or less than the count of the existing item in the registry and the effect would be that it would remove count number of items from the registered item. This change essentially only allows removeItem to remove any items from the registry that were not already bought. You could also simply not allow items to be removed that have already been purchased.
3) Based on the functionality, a registry can be opened and closed repeatedly. One reason this should be allowed is for when the registry is used for ongoing or recurring needs. For example sometimes people have registries as a means of charity, when all of the items needed have be bought, the registry can close, once more items are needed, the registry can open up again.
4) There being no action to delete a registry does not matter in practice, because the action of deleting a registry functionally means that the registry is now empty and closed, practically it means that users can no longer purchase items from the registry. This action can be done in practice simply closing the registry or functionally by deleting each item in the registry and then closing the registry..
5) Two common queries likely to be executed against the concept state: Registry owner might request what items have been purchased and a purchaser might request if a specific item has been purchased yet.
6) I would add a hideUser value to the set of Registries and a hideUser value to the create action. If the creator of the registry set hideUser to be true when creating a registry, they will not be able to see the identities of the purchaser.
7) Using SKU codes instead of names, descriptions, and prices is a better method because it allows for a unique and identifiable but still simple way to keep track of an Item. Names, descriptions, and price are all descriptors that could apply to many items or are too complicated to match to individual items.

## Exercise 2
1) a set of Users with<br>
    <ul>a username String<br>
      a password String<br>
     </ul>
2) register (username: String, password: String): (user: User)<br>
      <ul>
      <b>requires</b> username does not already exist in Users<br>
      <b>effects</b> creates a new user with this username and password and add it to set of Users <br>
      </ul>
      
   authenticate (username: String, password: String): (user: User)<br>
     <ul>
      <b>requires</b> username needs to exist within the set of Users, password to match with that Users recorded password<br>
      <b>effects</b> return that User<br>
    </ul>
3) the essential invariant is that all usernames must be unique, otherwise users could access a seperate users account. This is preserved in the register action by the fact that username is required to not already exist in the set Users

4) all listed items are only changed items, all other items from above not listed are expected to stay the same <br>
   **State**<br>
  a set of UnconfirmedUsers with<br>
    <ul>
      a username String<br>
      a password String<br>
      a token String<br>
    </ul>
   <b>Actions</b><br>
    register (username: String, password: String, email: Email): (user: UnconfirmedUser)<br>
      <ul>
      <b>requires</b> username does not already exist in Users or UnconfirmedUsers, email needs to be a vaild email<br>
      <b>effects</b> sends a confirmation email with a secret token to the email, creates new unconfirmed user with this username, password, and secret tokin, and add it to set of UnconfirmedUsers <br>
      </ul>
    confirm (username: String, token: String) : (user: User): <br>
      <ul>
        <b>requires</b> username needs to exist in UnconfiremdUsers, token needs to match token of that user. 
         <b>effects</b> create a new user with username and password and add it to Users, return User and remove this 
      </ul>

## Exercise 3
