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







## Title 

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.
```
code 
```

