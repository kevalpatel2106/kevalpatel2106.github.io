---
layout: post
title: Android Ringtone Picker
category: Open Source
image : https://kevalpatel2106.github.io/img/blog/ringtone-picker/image1.png
github : https://github.com/kevalpatel2106/android-ringtone-picker
---

Simple Ringtone Picker dialog which allows you to pick different sounds from ringtone, alarm tone, notification tone and music from external storage.

## **Gradle dependency:**

Add below dependency into your build.gradle file.

<code>compile 'com.kevalpatel2106:ringtonepicker:1.0'</code>

## **How to use?**

Use <code>RingtonePicker.Builder</code> to build the ringtone picker dialog.
Pass all the parameters and call <code>RingtonePicker.Builder#show()</code> to display ringtone picker dialog.

<code>
	RingtonePickerDialog.Builder ringtonePickerBuilder = new RingtonePickerDialog.Builder(MainActivity.this, getSupportFragmentManager());

	//Set title of the dialog.
	//If set null, no title will be displayed.
	ringtonePickerBuilder.setTitle("Select ringtone");

	//Add the desirable ringtone types.
	ringtonePickerBuilder.addRingtoneType(RingtonePickerDialog.Builder.TYPE_MUSIC);
	ringtonePickerBuilder.addRingtoneType(RingtonePickerDialog.Builder.TYPE_NOTIFICATION);
	ringtonePickerBuilder.addRingtoneType(RingtonePickerDialog.Builder.TYPE_RINGTONE);
	ringtonePickerBuilder.addRingtoneType(RingtonePickerDialog.Builder.TYPE_ALARM);

	//set the text to display of the positive (ok) button. (Optional)
	ringtonePickerBuilder.setPositiveButtonText("SET RINGTONE");

	//set text to display as negative button. (Optional)
	ringtonePickerBuilder.setCancelButtonText("CANCEL");

	//Set flag true if you want to play the com.ringtonepicker.sample of the clicked tone.
	ringtonePickerBuilder.setPlaySampleWhileSelection(true);

	//Set the callback listener.
	ringtonePickerBuilder.setListener(new RingtonePickerListener() {
	    @Override
	    public void OnRingtoneSelected(String ringtoneName, Uri ringtoneUri) {
	        //Do someting with Uri.
	    }
	});

	//set the currently selected uri, to mark that ringtone as checked by default. (Optional)
	ringtonePickerBuilder.setCurrentRingtoneUri(mCurrentSelectedUri);

	//Display the dialog.
	ringtonePickerBuilder.show();
</code>

## **Sample:**

<img align="middle" src="https://kevalpatel2106.github.io/img/blog/ringtone-picker/image2.gif" alt="demo.gif" width="240" height="427" />
