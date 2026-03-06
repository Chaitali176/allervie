---
description: Implement Liine Integration for Appointment and PPC Pages
---

## 1. Home Page / Primary Navigation
**Request:** The "Make an Appointment" button in the primary navigation should link directly to the online booking system (`https://portal.allerviehealth.com/abs`).
- **GLIDE Action Item:** Update the "Make an Appointment" link in the global Theme Options to point to the new URL (`https://portal.allerviehealth.com/abs`).
- **Dev approach:** No theme code changes required. The link update will be handled directly via the WordPress backend.
- **Allervie Action Item:** None.
- **Time Estimation:** 0.5 Hours

## 2. Make an Appointment Page
**Request:** Replace existing Gravity Form with Liine iFrame embed code.
- **GLIDE Action Item:** Adjust the page template to remove the existing Gravity Form block from the page frontend and add support to insert the "Liine iFrame" embed code dynamically.
- **Dev approach:** Clean up `template-appointment.php` to remove the Gravity Form shortcode block and any associated JS parsing the URL parameters. Implement ACF fields to allow the client to drop the new Liine iframe on the Appointment page backend, then output this on the frontend in `template-appointment.php`.
- **Allervie Action Item:** Provide the general Liine iFrame embed code.
- **Time Estimation:** 2 Hours

## 3. PPC Page Templates
**Request:** Each PPC page requires its own individual Liine iFrame embed code.
- **GLIDE Action Item:** Enhance the PPC Template Options to support adding a custom Liine iFrame embed code per page. Implement this on one designated page for review and approval.
- **Dev approach:** Modify `template-ppc.php` and its associated ACF fields to allow unique iFrame code to replace the Gravity Form code. We will implement it conditionally so that the provided iFrame appears over the standard Gravity form if filled out. Test this on one page first.
- **Allervie Action Item:** Confirm if each PPC page will have a unique code. After structure setup, Allervie team will manage the transition for the remaining PPC pages with their unique iFrame codes.
- **Time Estimation:** 2.5 Hours

## 4. Location Pages
**Request:** "Submit a Request" -> `https://www.allervie.com/make-an-appointment/`, "Book Online" -> `https://portal.allerviehealth.com/abs`.
- **GLIDE Action Item:** No direct dev action required from Glide.
- **Dev approach:** Parameter-based URLs are already removed. Location pages currently jump to different sections. Buttons are managed in the backend per location via Single Location Options.
- **Allervie Action Item:** Manually update the "Submit a Request" and "Book Online" links across all 62 published Location Pages via the backend as per instructions.
- **Time Estimation:** 0 Hours
