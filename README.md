# Finder Module

CREATED BY: CORNELL DATA MANAGEMENT SERVICE GROUP AND CORNELL INFORMATION TECHNOLOGIES CUSTOM DEVELOPMENT GROUP

This software is being shared free of cost and with no restrictions on re-use or modification. The code is provided “as is” without warranty of any kind, express or implied, and Cornell University takes no responsibility for maintenance or upgrades.

If you chose to re-use the code, please cite the original creators as: Cornell University Research Data Management Service Group and Cornell Information Technologies Custom Development Group (2018). Finder Module. Dupal 8. https://github.com/CU-CommunityApps/CD-finder

## Overview

The purpose of the Finder module is to help users choose among a number of similar related alternatives.

In our initial use case, we had 18-20 computer data storage alternatives available for researchers. Our Help Desk created a list of questions to guide the researchers to specify their requirements. The Finder module allowed the researchers to enter requirements and interactively narrow the field of 18-20 to perhaps 2-3 alternatives.
After the users clicks checkboxes to enter their criteria, ineligible results are greyed out. The user can click on remaining eligible results; these are highlighted and detailed characteristics of this smallest group are presented in a table to allow the researchers to make their final choice.

Implementing this application in Drupal allows the service managers to easily edit the available services and supporting data without additional programming.

We believe other groups will have a similar need for a self-service product to guide end-users to narrow down a choice within a set of moderately complex alternatives.  

## Planning

When using this module to help end-users choose between a number of complex alternatives, based on a number of criteria and the evaluation of information in several categories, expect that gathering the supporting information and designing useful questions will take a significant amount of planning and time. Changing questions or answers or adding or deleting categories of information after services have been entered requires editing each of the services.  

We recommend starting with a simple example like that presented below to familiarize yourself with the data layout and editing workflow before beginning a “real” project.

## Install Finder module via Composer
### Install Drupal 8 using Composer:

* composer create-project drupal-composer/drupal-project:8.x-dev testdrupal 

### Require the Finder module:

* cd  testdrupal
* composer  require nyuhsl/finder

Note: If you aren’t using composer to manage your Drupal 8 site, you can clone the module from: https://github.com/CU-CommunityApps/CD-finder

From the Extend administrative page (/admin/modules), install the Finder module and its dependencies. 

## Setup of the Finder Module

* In the Control Type taxonomy (/admin/structure/taxonomy/manage/control_type/overview)  add two terms: "checkbox" and "radio.".
* In the Facets taxonomy create a two-level hierarchy of terms representing criteria/questions and choices. For each of the criteria (top level facets), specify a control type: either “checkbox” (used when any, all or none of the choices may be selected) or “radio” (used when only one of the choices may be selected).
* Drag the Facet taxonomy terms into a two level hierarchy, representing the relationship between criteria/questions and choices.

Services (the things being chosen) can contain a lot of data, and so require a bit of planning. Once the end-user has narrowed down their choices, they will be presented with a comparison table containing data in several fields. These fields are represented by a  Drupal Paragraph called “Service Paragraphs”. Service Paragraphs should be edited before you start entering Service data at 
/admin/structure/paragraphs_type/service_paragraphs/fields 

When delivered, the Finder module comes with two fields in Service Paragraphs: “First Category” and “Second Category.” You can rename or replace these, and add as many fields as you wish. We have found that Text (formatted, long) fields work best.

Now you may add Services. From the images above, you can see that a service has a title, summary (show on the cards in the first image. They are “eligible” to be chosen, depending on the choices the end-user has specified, and so need a set of “facet-matches” to indicate whether they are compatible with specific choices.  They also have text data in several categories to be able to populate the comparison table.

Create your Services using the Drupal Service content-type. Add title and summary for each.
Then fill in the text for the Service Paragraphs you have chosen.

Required:  Add an additional Service with the title “Help” and put text in each field describing what to expect in that field. This will be used by the Finder module to display help information for the each row in the comparison table. The comparison table may not appear if the Help Service is not defined.

## Tips

* You may wish to remove all sidebar blocks from this page, so that the Finder can use the full width of the page.
* The Finder module is displayed at the URL path /finder.
* There is a configuration page to edit titles and other data on the Finder page at /admin/config/content/finder
* Check out our production implementation at https://finder.research.cornell.edu
* You will need to configure the SMTP module to allow the Finder module to send mail.
* Installation works only at a root site, not if installed in a subdirectory.

## Drupal 9 changes

Here are some notes that may be useful if you wish to use this module with Drupal 9. 

```
src/Controller/StorageController.php line 231: Call to deprecated method entityManager() of class Drupal. Use Drupal::entityTypeManager() instead in most cases.If the needed method is not on \Drupal\Core\Entity\EntityTypeManagerInterface, see the deprecated \Drupal\Core\Entity\EntityManager to find the correct interface or service. 
The corrected line 231 is:
  $field_config = \Drupal::entityTypeManager()->getStorage('field_config')->load("paragraph" . '.' . "service_paragraphs" . '.' . $machine_name)->toArray();
```

```
src/Controller/StorageController.php lines 364, 368:
Call to deprecated function drupal_set_message().
Use Drupal\Core\Messenger\MessengerInterface::addMessage() instead.
Line 364: \Drupal::messenger()->addMessage($this->t('There was a problem sending your message and it was not sent.'), 'error');
Line 368: \Drupal::messenger()->addMessage($this->t('Your message has been sent.')); 
```

``
storage.info.yml: Add core_version_requirement: ^8 || ^9
``

```
For PHP 8: same file, at/nearline 361: implode($facets,",") . becomes implode(",",$facets) .
```
