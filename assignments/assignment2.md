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
        <b>requires</b> username needs to exist in UnconfiremdUsers, token needs to match token of that user. <br>
         <b>effects</b> create a new user with username and password and add it to Users, return User and remove this 
      </ul>

## Exercise 3
<b>concept</b> PersonalAccessToken\[User\]<br>
<b>purpose</b> an alternative to passwords for authentication to GitHub that allows for multiple tokens to be made for the same account<br>
<b>principle</b> A user generates a personal access token that is now associated with that user. Users provide their username and token to be authenticated, a user can use any of the tokens currently related to that User. New tokens can be added at any time and individual tokens can be removed at any time.<br>
<b>state</b><br>
a set of Users with<br>

<ul> a username String<br> a set of Tokens with<br> <ul> a tokenName String<br> an active Flag<br> </ul> </ul>

<b>actions</b><br>
generateToken (user: User) : (token: Token)<br>

<ul> <b>requires</b> user exists in set of Users<br> <b>effects</b> creates a new Token with a randomly generated tokenName, marks token as active, adds it to the set of Tokens of the specified user, and returns Token<br> </ul> revokeToken (user: User, token: Token)<br> <ul> <b>requires</b> user exists in set of Users, token exists within the set of Tokens associated with the user, and the token is active<br> <b>effects</b> marks the specified token as inactive<br> </ul> authenticate (username: String, tokenName: String) : (user: User) <br> <ul> <b>requires</b> username is associated with a User in the set of Users, tokenName is associated with an active token in the set of Tokens for that user<br> <b>effects</b> returns that User<br> </ul>

**difference** The difference between a standard password and a token system is that with a password only one password is associated with that account. However, with a token system there can be several tokens associated with one account. This allows for safer automation and integration with external systems, and also means that if a token is compromised, it can be revoked without affecting the other tokens or the account password. 

## Exercise 4
**Billable Hours Tracking**
<b>concept</b> BillableHours\[Client\]<br>
<b>purpose</b> Track billing hours for different clients<br>
<b>principle</b> Allow employees to log when they start working on a project and what they are doing for that project. When they are finished with that session of work, they log the ending time. When the end time is logged, the system logs the hours from that session to the bill of the client that project is for. If an employee forgets to log off, then they are able to retroactively log when they finished the session<br>
<b>state</b><br>
a set of Clients with<br>
<ul>
    a name String<br>
    a totalBillableHours Number<br>
</ul>
a set of projects with<br>
<ul>
    a name String<br>
    a client Client<br>
    a billableHours Number<br>
</ul>
a set of OngoingSessions with <br>
<ul>
    a employeeId Employee<br>
    a startTime Time<br>
    a description String<br>
    a project Project<br>
</ul>
a set of FinishedSessions with <br>
<ul>
    a employeeId Employee<br>
    a startTime Time<br>
    a endTime Time<br>
    a description String<br>
    a project Project<br>
</ul>
    
<b>actions</b><br>
addClient(name: String): (client: Client)<br>
<ul>
    <b>requires</b> name does not match any name already existing within the set of clients<br>
    <b>effects</b> creates new client with name as its name and initializes totalBillableHours to be zero<br>
</ul>
createProject(name: String, client: Client): (project: Project)<br>
<ul>
    <b>requires</b> name does not exist in set of Projects, client exists in set of Clients<br>
    <b>effects</b> creates new Project, initializes BillableHours to be zero<br>
</ul>
startSession(project: Project, description: String, startTime: Time): (session: OngoingSession)
<ul>
    <b>requires</b> project exists, no active ongoing session for this project by same employee<br>
    <b>effects</b> create new OngoingSession<br>
</ul>
endSession(session: OngoingSession, endTime: Time): (finished: FinishedSession)
<ul>
    <b>requires</b> session exists, endTime > startTime<br>
    <b>effects</b> move session to FinishedSessions, update project.billableHours and client.totalBillableHours<br>
</ul>
retroEndSession(session: OngoingSession, endTime: Time): (finished: FinishedSession)
<ul>
    <b>requires</b> session exists, endTime > startTime<br>
    <b>effects</b> same as endSession, but allows closing later<br>
</ul>
finishProject(project: Project): (hours: Number)
<ul>
    <b>requires</b> project exists<br>
    <b>effects</b> return project.billableHours<br>
</ul>
getBillableHours()
<ul>
    <b>requires</b> <br>
    <b>effects</b> <br>
</ul>
getTotalBillableHours(client: Client): (hours: Number)
<ul>
    <b>requires</b> client exists<br>
    <b>effects</b> return client.totalBillableHours<br>
</ul>

**Notes** The ongoing vs finishes sessions are to seperate which sessions have already been billed, projects are seprated from simply adding to the totalBillableHours due to the possibility of one company having several ongoing projects. The use of User for start and end session allows several different people to work on the same project at the same time, but prevents one employee from counting the same session several times. 

**Confrence Room Booking**
<b>concept</b> ConferenceRoomBooking<br>
<b>purpose</b> Enable individuals to reserve conference rooms for meetings, ensuring no double-bookings.<br>
<b>principle</b> A user selects a room and time interval, and if available, books it. Reservations can be canceled. Rooms cannot be double-booked.<br>
<b>state</b><br>
a set of Rooms with<br>
<ul>
    a name String<br>
</ul>
a set of Bookings with<br>
<ul>
    a room Room<br>
    a startTime Time<br>
    a endTime Time<br>
    a user User<br>
</ul>

<b>actions</b><br>
addRoom (name: String): (room: Room)
<ul>
    <b>requires</b> name not already in Rooms<br>
    <b>effects</b> create and return new Room<br>
</ul>

bookRoom (room: Room, user: User, startTime: Time, endTime: Time): (booking: Booking)
<ul>
    <b>requires</b> room exists, endTime > startTime, no overlapping booking in same room<br>
    <b>effects</b> create Booking and add to Bookings<br>
</ul>

cancelBooking (booking: Booking)
<ul>
    <b>requires</b> booking exists<br>
    <b>effects</b> remove booking from Bookings<br>
</ul>

**Notes** The essential invariant is that no two bookings for the same room overlap in time. The bookRoom action checkes this. 
**Time-Based One-Time Password (TOTP)**
<b>concept</b> TimeBasedOneTimePassword<br>
<b>purpose</b> Improve authentication security by requiring users to provide a short-lived, time-based token in addition to a username and password.<br>
<b>principle</b> Each user is registered with a secret key. A token generator produces codes from this key and the current time. During authentication, the user provides their username, password, and token; if the token matches the expected value, access is granted.<br>
<b>state</b><br>
a set of Users with <br>
<ul>
    a username String
    a password String
    a token String
    a expirationTime Time
    a pendingAuth Flag
</ul>
<b>actions</b><br>
register (username: String, password: String, secretKey: String): (user: User)
<ul>
    <b>requires</b> username not already in Users<br>
    <b>effects</b> create new User with password and connects to application<br>
</ul>
login (username: String, password: String): (pendingAuth: User)
<ul>
    <b>requires</b> username exists, password matches<br>
    <b>effects</b> return user in a pending-authentication state, add token adn expirationTime<br>
</ul>
authenticate (user: User, token: String): (user: User)
<ul>
    <b>requires</b> user in pending-authentication state, token matches expected value from user.secretKey and current time window<br>
    <b>effects</b> Return user, set pendingAuth to false<br>
</ul>

**Note** Register actions creates new account and attaches it to the application. Login creates the temporary password and sets user as pendingAuth. Authorization occurs when the user enters the correct, nonexpired, token. 
