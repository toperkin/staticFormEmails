# staticFormEmails
How to put forms which send emails on action into static sites without redirect

## step 1 - make a simple html form.

```html
<form>
  First name:<br>
  <input type="text" name="firstname"><br>
  Last name:<br>
  <input type="text" name="lastname">
  <input type="submit" value="Submit">
</form>
```

## step 2 - make a google form with the same fields

![alt text](https://github.com/toperkin/staticFormEmails/raw/master/newForm.png "Google Form")

## step 3 - We need to know what google calls these fields.  Make a pre-filed form, fill with anything, and grab the link.

![alt text](https://github.com/toperkin/staticFormEmails/raw/master/prefilled.png "Pre-filled link")

![alt text](https://github.com/toperkin/staticFormEmails/raw/master/getlink.png "Get link")

```
https://docs.google.com/forms/d/14lh6MIQOy9j3jWzEl7BysxEe4p9OoU9WN3tytbQjj1I/viewform?entry.810989529=garbageFirstName&entry.463380756=garbageLastName
```

## step 4 - use the entry fields from the pre-filled link for your form name/ids, and set the action to direct to your google form.  Also add some hidden_iframes, this is used to block the redirect.

```html
<form name="gform" id="gform" enctype="text/plain" action="https://docs.google.com/forms/d/14lh6MIQOy9j3jWzEl7BysxEe4p9OoU9WN3tytbQjj1I/formResponse" target="hidden_iframe" onsubmit="submitted=true;">
  First name:<br>
  <input type="text" name="entry.810989529" id="entry.810989529"><br>
  Last name:<br>
  <input type="text" name="entry.463380756" id="entry.463380756">
  <input type="submit" value="Submit">
</form>

<iframe name="hidden_iframe" id="hidden_iframe" style="display:none;" onload="if(submitted) {}"></iframe>
```

## step 5 - add some javascript to keep track of what happens after a submit

```html
<script src="assets/js/jquery.min.js"></script>
<script type="text/javascript">var submitted=false;</script>
<script type="text/javascript">
$('#gform').on('submit', function(e) {
  $('#gform *').fadeOut(2000);
  $('#gform').prepend('Your submission has been processed...');
  });
</script>
```

That's it.  You now have an html form that upon submit completes the google form (which can easily be set up to populate a spreadsheet and/or email you), but DOES NOT REDIRECT, instead just fades away and is replaces with a friendly message.
