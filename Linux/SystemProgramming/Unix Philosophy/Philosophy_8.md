# Provide the mechanism, not the policy

- Let's say we are designing an application where we need to have the user enter a login `name` and `password`.
- The protocol to be followed is this: if the user gets the password wrong three times in a row, the program should not allow access(and should log the case).
- **The Unix Philosophy**: do not implement the logic, if the password is specified wrongly three times, abort in the `mygetpass()` function. Instead, just have `mygetpass()` return a Boolean (true when the password is right, false when the password is wrong), and have the `mylogin()` calling function implement whatever logic is required.

## Pseudocode

The following is the wrong approach.

```css
mygetpass()
{
	numtries=1
	<get the password>
	if (password-is-wrong) {
		numtries ++
		if (numtries >= 3) {
			<write and log failure message>
			<abort>
			}
		}
<password correct, continue>
}

mylogin(){
    mygetpass()
}
```

The right approach: The Unix way!

```css
mygetpass()
{
	<get the password>
	if (password-is-wrong)
		return false;
	return true;
}
mylogin()
{
	maxtries = 3
	while (maxtries--) {
		if (mygetpass() == true)
		<move along, call other routines>
	}
	// If we're here, we've failed to provide the
	// correct password
	<write and log failure message>
	<abort>
}
```

- The job of `mygetpass()` is to get a password from the user and check whether it's correct; it returns success or failure to the caller - that's it. That's the **mechanism**.
- It's not its job to decide what to do if the password is wrong - that's the **policy** and left to the caller.
