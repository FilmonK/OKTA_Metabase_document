# Expanding SAML + Okta Integration in Metabase

## Adding User Attributes for Data Sandboxing and Row-Level Permissions

### ✴️ Overview

This guide expands on [Metabase’s SAML + Okta documentation](https://www.metabase.com/docs/latest/people-and-groups/saml-okta), specifically demonstrating how to configure custom user attributes in Okta and pass them into Metabase for use with [data sandboxing](https://www.metabase.com/learn/metabase-basics/administration/permissions/data-sandboxing-row-permissions) and [impersonation](https://www.metabase.com/learn/metabase-basics/administration/permissions/impersonation).

You’ll learn how to:
- Add custom user attributes in Okta.
- Pass those attributes via SAML into Metabase.
- Confirm that attributes are received correctly in Metabase.
- Use those attributes for features like sandboxing and impersonation.

---

### ✴️ Prerequisites

- Metabase Enterprise Edition with SAML authentication enabled.
- An Okta admin or developer account.
- A configured SAML connection between Okta and Metabase.
- An attribute you want to bring over into Metabase (e.g., `region`, `department`, or `database_role`).

---

### ✴️ Example Use Case

- A user logs into Metabase via Okta.
- Their Okta profile includes `region = "US-West"`.
- The `region` attribute is passed into Metabase via SAML.
- The attribute is used by sandboxing or impersonation (depending on how Metabase is configured).

---

### ✴️ What is Data Sandboxing in Metabase?

In Metabase, **data sandboxing** allows admins to restrict what rows of data a user can see within a shared table, based on their group or identity. For example, you might allow users to query a full `orders` table but only return rows where `customer_id` matches their assigned ID. This is especially important in multi-tenant setups where users from different companies or teams share the same schema but must only access their own data.   

[Learn more about Data Sandboxing →](https://www.metabase.com/learn/metabase-basics/administration/permissions/data-sandboxing-row-permissions)

---

### ✴️ What is Impersonation in Metabase?

**Impersonation** in Metabase allows the application to pass the identity of the currently logged-in user—such as their email, group, or custom attributes—directly to the database when executing queries. This enables **dynamic row-level security** at the database level, rather than relying solely on Metabase’s internal permissions.  

[Learn more about Impersonation →](https://www.metabase.com/learn/metabase-basics/administration/permissions/impersonation)

---  

### ✴️ Step 1: Add Custom Attributes to Okta User Profiles

1. In Okta, go to **Directory → Profile Editor**.
2. Find the correct profile to modify and click **Add Attribute**.
3. Add any desired fields, such as:
   - `region`
   - `department`
   - `database_role`
4. Click **Save**.
5. The attribute should now appear along with the other user fields.
<img src="https://github.com/FilmonK/OKTA_Metabase_document/blob/main/readme_media/okta_attribute.png?raw=true" width="600" alt="Okta Attribute" />

---

### ✴️ Step 2: Assign Attribute Values

1. In Okta, go to **Directory → People → A user account → Profile → Edit**.
2. Assign a value to any of the attributes that were created  
   (e.g. `database_role` → `sales`)

---

### ✴️ Step 3: Add SAML Attribute Statements in Okta

1. In Okta, go to **Applications → Your Metabase App → General**.
2. Click **Edit** in the **SAML Settings** section.
3. Scroll to **Attribute Statements** and add attributes like the following:

| Name           | Name format | Value               |
|----------------|-------------|---------------------|
| `region`       | Unspecified | `user.region`       |
| `department`   | Unspecified | `user.department`   |
| `database_role`| Unspecified | `user.database_role`|

4. Click **Next**, then **Finish**.
5. Confirm it's working by clicking on **Preview the SAML Assertion** in the same screen.
<img src="https://github.com/FilmonK/OKTA_Metabase_document/blob/main/readme_media/saml.png?raw=true" width="600" alt="SAML Settings" />

---

### ✴️ Step 4: Log into Metabase to Confirm Attributes

1. Log in using SSO (Okta).
2. Go to **Admin Settings → People**.
3. Click on the 3-dot button to edit the user.
4. The attributes should be displayed.
<img src="https://github.com/FilmonK/OKTA_Metabase_document/blob/main/readme_media/user_metabase.png?raw=true" width="600" height="800" alt="Metabase User Attributes" />

These attributes can now be used within Metabase for sandboxing or impersonation depending on how you’ve configured those features.


---

### ✴️ Troubleshooting Tips

- ✅ Use a SAML trace tool to inspect which attributes are included in the SAML assertion.
- ✅ Attribute names are case-sensitive. Confirm Okta and Metabase match exactly.
- ✅ You can test mappings by creating a dummy user in Okta with test values.
- ✅ For impersonation issues, confirm the target database supports user-based impersonation.

---

### ✴️ Additional Resources

- [Metabase SAML + Okta Docs](https://www.metabase.com/docs/latest/people-and-groups/saml-okta)
- [Metabase Data Sandboxing](https://www.metabase.com/learn/metabase-basics/administration/permissions/data-sandboxing-row-permissions)
- [Metabase Impersonation](https://www.metabase.com/learn/metabase-basics/administration/permissions/impersonation)
- [SAML Tracer Tool (Chrome)](https://chromewebstore.google.com/detail/saml-tracer/mpdajninpobndbfcldcmbpnnbhibjmch?hl=en)
