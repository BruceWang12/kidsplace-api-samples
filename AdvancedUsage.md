#Advanced Usage Guide

# Introduction #

This is Kids Place API Advanced Usage Guide. You need to set up your project as describe in [basic setup](http://code.google.com/p/kidsplace-api-samples/#basic_setup) before using all the features described here.

If you have any question or comment regarding ACRA, post a message on the [Discussion Group](http://groups.google.com/group/kids-place-sdk-group).

For sample source code for advanced integration please refer [advanced](http://code.google.com/p/kidsplace-api-samples/source/browse/#svn%2Ftrunk%2Fsamples%2Fadvanced) project code.

# User Interaction #

  * Provide a setting in your app to enable child lock. This will give user an option to choose if they would like to childproof your Android app. Add to your preferences xml file a CheckBoxPreference (checking it enables child lock) :
```
<CheckBoxPreference android:key="childLockEnabled"
			android:summary="@string/childLockSummary"
			android:title="@string/childLockTitle" android:defaultValue="false" />
```
  * Before starting Kids Place app, display a simple dialog asking them if they would like to start Kids Place. This will make sure that even if they have enabled the child lock settings, they can still choose to not run Kids Place from your app.
```
	private void showStartKPMessage() {

		if (startKPdlg != null && startKPdlg.isShowing()) {
			startKPdlg.dismiss();
			startKPdlg = null;
		}
		startKPdlg = new AlertDialog.Builder(this)
				.setTitle(R.string.startKPTitle)
				.setMessage(R.string.startKPMsg)
				.setPositiveButton(R.string.yesBtn,
						new DialogInterface.OnClickListener() {

							public void onClick(DialogInterface dialog,
									int whichButton) {
								try {
									dialog.dismiss();
									Utility.handleKPIntegration(getActivity());

								} catch (Exception ex) {

								}
							}
						})
				.setNegativeButton(R.string.cancelBtn,
						new DialogInterface.OnClickListener() {
							public void onClick(DialogInterface dialog,
									int whichButton) {
								// Canceled.
								try {
									dialog.dismiss();
									Utility.setRunningStandAlone(true);
								} catch (Exception ex) {

								}
							}
						}).show();
	}
```

# Limiting access to kids from certain areas of app #
  * You can prevent certain part of your app like settings; in-app purchases etc by password protecting it so that only parents can access it. Kids Place API makes it easy for app developers and parents by authenticating against PIN already set up by parents for kids. This prevents app developers for implementing an authentication infrastructure and for parents to remember multiple PIN.

  1. Create new layout for PIN prompt
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:id="@+id/toast_layout_root" android:layout_width="fill_parent"
	android:layout_height="wrap_content" 
	android:orientation="vertical">
	<EditText android:layout_width="fill_parent"
		android:layout_height="wrap_content" android:id="@+id/pin"
		android:password="true" android:numeric="integer" android:maxLength="4" />
	<LinearLayout android:layout_width="fill_parent"
		android:layout_height="wrap_content" android:orientation="horizontal">
		<TextView android:text="@string/pin_hint_label2" android:id="@+id/textView1" 
			android:layout_width="wrap_content" android:layout_height="wrap_content"
			android:layout_marginRight="2.0sp">
		</TextView>
		<TextView android:layout_width="fill_parent"
			android:layout_height="wrap_content" android:id="@+id/pin_hintTextView"
			android:text="" />
	</LinearLayout>


	<TextView
	    android:id="@+id/textViewForgotPin"
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:layout_marginTop="3dp"
	    android:text="@string/forgotPin"
	    android:textSize="12.0sp" />

</LinearLayout>
```
  1. Add following method to the activity from where you want to prompt user to enter for PIN.
```
	private void showPasswordFields() {
		passwordDialog = null;
		final AlertDialog.Builder alert = new AlertDialog.Builder(this);
		Context mContext = getApplicationContext();

		LayoutInflater inflater = (LayoutInflater) mContext
				.getSystemService(LAYOUT_INFLATER_SERVICE);
		View layout = inflater.inflate(R.layout.enter_pin, null);
		alert.setTitle(R.string.pin_request);
		alert.setMessage(R.string.pin_message);
		// Set an EditText view to get user input
		final EditText input = (EditText) layout.findViewById(R.id.pin);
		input.setInputType(InputType.TYPE_CLASS_PHONE);
		input.setTransformationMethod(PasswordTransformationMethod
				.getInstance());
		alert.setView(layout);
		final TextView pinHintText = (TextView) layout
				.findViewById(R.id.pin_hintTextView);
		pinHintText.setText(KPUtility.getPinHint(getApplicationContext()));
		alert.setPositiveButton("Ok", new DialogInterface.OnClickListener() {

			public void onClick(DialogInterface dialog, int whichButton) {
				String value = input.getText().toString();
				if (KPUtility.validatePin(getApplicationContext(), value)) {
                                //Valid PIN so take user to desired activity/action
					Toast.makeText(getApplicationContext(),
							"Authorization Successful",
							Toast.LENGTH_SHORT).show();					

                               //IMPLEMENT SUCCESS USE CASE  HERE
						

				} else {
					// hide the password layout
					Toast.makeText(getApplicationContext(),
							R.string.incorrect_pin, Toast.LENGTH_SHORT).show();
				}
			}
		});

		alert.setNegativeButton("Cancel",
				new DialogInterface.OnClickListener() {
					public void onClick(DialogInterface dialog, int whichButton) {
						// Canceled.
					}
				});

		passwordDialog = alert.create();
		passwordDialog.getWindow().setGravity(Gravity.TOP);
		input.setOnFocusChangeListener(new View.OnFocusChangeListener() {
			@Override
			public void onFocusChange(View v, boolean hasFocus) {
				if (hasFocus) {
					if(passwordDialog != null){
					passwordDialog
							.getWindow()
							.setSoftInputMode(
									WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_VISIBLE);
					}
				}
			}
		});
		passwordDialog.show();
		// auto close password dialog after 20 seconds
		final Timer t = new Timer();

		t.schedule(new TimerTask() {

			public void run() {
				try {
					passwordDialog.dismiss();
				} catch (Exception ex) {

				}
				t.cancel(); // also just stop the timer thread, otherwise, you
							// may receive a crash report

			}

		}, 20000); //
	}	
```