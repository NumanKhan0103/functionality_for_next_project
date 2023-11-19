# Functionality For My Next Laravel Project



## Preventing Duplicate Form Submissions Using Atomic Locks

code with out lock 

```bash
class SendPaymentController
{
    public function __invoke(Request $request)
    {
        /* validate the request */

        $account = $request->user()->accounts()->findOrFail($request->input('account'));

        $recipient = Account::findOrFail($request->input('recipient'));

        $amount  = $request->input('amount');

        /* process the request */

        return to_route('payments.create')
            ->with('status', [
                'type' => 'success',
                'message' => 'You have successfully sent a payment of '.number_format($request->input('amount'), 2).' to '.$recipient->name.'.',
            ]);
    }
}
```



Code With Lock

```python
public function __invoke(Request $request)
{
    /* validate the request */

    $account = $request->user()->accounts()->findOrFail($request->input('account'));

    $recipient = Account::findOrFail($request->input('recipient'));

    $amount  = $request->input('amount');

    $lock = Cache::lock($account->id.':payment:send', 10);

    if (! $lock->get()) {
        return to_route('payments.create')
            ->with('status', [
                'type' => 'error',
                'message' => 'There was a problem processing your request.',
            ]);
    }

    /* process the request */

    return to_route('payments.create')
        ->with('status', [
            'type' => 'success',
            'message' => 'You have successfully sent a payment of '.number_format($request->input('amount'), 2).' to '.$recipient->name.'.',
        ]);
}
```

In the above code, we are creating a lock with the name {$account->id}:payment:send which will be valid for 10 seconds. If the lock is acquired, we will process the request and redirect the user back to the form with a success message. If the lock is not acquired, we will redirect the user back to the form with an error message.

The error message is optional, you can just redirect the user back to the form without any message but for the sake of this example, we are showing an error message.

🎉🎉🎉🎉🎉🎉 That’s it! we have now implemented atomic locks to prevent duplicate form submissions.🎉🎉🎉🎉🎉🎉
For More info  https://daryllegion.com/preventing-duplicate-form-submissions-using-atomic-locks







# Laravel Hashing For Strong Passwords 
## Adjusting The Bcrypt Work Factor
If you are using the Bcrypt algorithm, the make method allows you to manage the work factor of the algorithm using the rounds option; however, the default work factor managed by Laravel is acceptable for most applications:

```
$hashed = Hash::make('password', ['rounds' => 12,]);
```

## Adjusting The Argon2 Work Factor
If you are using the Argon2 algorithm, the make method allows you to manage the work factor of the algorithm using the memory, time, and threads options; however, the default values managed by Laravel are acceptable for most applications:
```
$hashed = Hash::make('password', ['memory' => 1024, 'time' => 2, 'threads' => 2]);

```

Check if a password needs rehashing with Hash::needsRehash during authentication.
```
if (Hash::needsRehash($hashed)) {
    $hashed = Hash::make('plain-text');
}
```

weekly backup of the data base 
get the code backup file every month with git command that numan use it



## Title 
## Title 
## Title 
 

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.
```
code 
```

