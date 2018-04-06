# LeadToHubspot
Module for ProcessWire that saves contacts in the Hubspot CRM (e.g. from a leadform).

# How To Install
1. Download the [zip file](https://github.com/danielstieber/LeadToHubspot/archive/master.zip) at Github or clone directly the repo into your `site/modules`
2. If you dowloaded the `zip file`, extract it in your `sites/modules` directory
3. Goto the modules admin page, click on refresh and install it.

# API
You must create an API key in your Hubspot account. To get your API-Key, log into hubspot.com, click on your avatar in the top right corner and go to "Integrations / Hubspot API-Key". Add the API key in the moduel settings page.

![module settings](https://i.imgur.com/bGfBU49.png)

# Usage
1. Read the restrictions
2. Call the module: ´$hubspot = $modules->get("LeadToHubspot");´
3. Call function saveLead and pass your data array: ´$hubspot->saveLead(['email' => 'john.doe@example.com', 'firstname' => 'John', 'lastname' => 'Doe'])´

# Important Notes
## Functionality
The module creates or updates a lead based on the transmitted data array. The field 'email' works as identifier. 

## Restrictions in this version
* The data **must contain a key 'email'** (it is used as identifier).
* Every key in the data array, **must be a field in your CRM properties**. You can find and overview of the default or create additional properties at hubspot.com -> Settings / Properties).

** I strongly recommend a server-sided form validation like [Valitron](https://github.com/vlucas/valitron) **

# Example Code
Example data array:
```PHP
	$data = [
		'email' => 'john.doe@example.com',
		'firstname' => 'John',
		'lastname' => 'Doe',
		'company' => 'ExampleCompany Ltd.',
		'phone' => '123456789'
	];
```

Example usage after form post:
```PHP
	// ... validation of form data
	$data = [
		'email' => $input->post->email,
		'firstname' => $input->post->firstname,
		'lastname' => $input->post->lastname,
		'message' => $input->post->message,
	];
 	$lead = array_map('trim', $lead);
 	if($hubspot->saveLead($lead))
 		echo "Thank you for your Message";
 	else
 		echo "Oops, an error accured while transmitting your data. We are sorry for the inconvinience. For your own security, your data has not been saved. Why not contact us directly at office@companyemail.com and we have talk about your request, while our IT-team is fixing the problem?";
?>
```

# Troubleshooting (check your ProcessWire warning & error logs)
##  Hubspot Lead not saved: 400 Bad Request: ...
The data you pass is not valid for a creating/update a contact in hubspot. This mostly happens when you use non-existing properties.

* 1. Check for typos in your data array
* 2. Check, if properties exist in Hubspot
* 3. Unquote the line 'die(print_f(json_encode($param)));' in site/modules/LeadToHubspot.module and check if the output is valid JSON format
