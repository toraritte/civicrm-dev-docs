# hook_civicrm_emailProcessorContact

## Summary

This hook is called by the Email Processor when deciding to which
contact and activity will be attached.

## Notes

You can use this hook to choose a different
contact or decide whether it should create contacts.

## Definition

    hook_civicrm_emailProcessorContact( $email, $contactID, &$result )

## Parameters

-   @param string $email     the email address
-   @param int      $contactID the contactID that matches this email
    address, IF it exists
-   @param array  $result (reference) has two fields
-                                                contactID - the new (or
    same) contactID
-                                                action - 3 possible
    values:
-
    CRM_Utils_Mail_Incoming::EMAILPROCESSOR_CREATE_INDIVIDUAL -
    create a new contact record
-                                                               CRM_Utils_Mail_Incoming::EMAILPROCESSOR_OVERRIDE
    - use the new contactID
-
    CRM_Utils_Mail_Incoming::EMAILPROCESSOR_IGNORE   - skip this
    email address\
     \

## Returns

-   null

## Availability

This hook was first available in CiviCRM 4.1.0

## Example

See civitest.module.sample.