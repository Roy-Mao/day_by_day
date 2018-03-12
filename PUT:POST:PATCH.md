# The differences between PUT/POST/DISPATCH and the concept of idompotence


Or from the other side of the fence: PUT if the client determines the resulting resource's address, POST if the server does it.

What I find the most valuable here is the note at the end, stating that a PUT should be used only for replacing the whole resource.

HTTP says ... when a client 'puts' to a url, it is requesting that url to hold that data. It's up to the server to decide if it's happy to do that.

PUT means "I am trying to store this here"

I like your statement, "Update can only be performed with PUT"


POST means "create new" as in "Here is the input for creating a user, create it for me".

PUT means "insert, replace if already exists" as in "Here is the data for user 5".

You POST to example.com/users since you don't know the URL of the user yet, you want the server to create it.

You PUT to example.com/users/id since you want to replace/create a specific user.

POSTing twice with the same data means create two identical users with different ids. PUTing twice with the same data creates the user the first and updates him to the same state the second time (no changes). Since you end up with the same state after a PUT no matter how many times you perform it, it is said to be "equally potent" every time - idempotent. This is useful for automatically retrying requests. No more 'are you sure you want to resend' when you push the back button on the browser.

A general advice is to use POST when you need the server to be in control of URL generation of your resources. Use PUT otherwise. Prefer PUT over POST.

Best part: "POSTing twice with the same data means create two identical [resources]". Great point!

For developers building REST-based APIs, there is a great deal of misinformation and some understandable confusion about when to use HTTP PUT and when to use HTTP POST.

To quote Dino Chiesa, “PUT implies putting a resource – completely replacing whatever is available at the given URL with a different thing.” With PUT requests, you MUST send all the available properties/values, not just the ones you want to change. If we were to send the status of “disabled” in addition to the givenName and surname, the call would be idempotent and eliminate the side effects. Idempotency is a fundamental property of the HTTP specification and must be adhered to to guarantee web interoperability and scale.

greate post as weather to use put or post: https://stormpath.com/blog/put-or-post

