Introduction
============

We currently have two environments, development and production. Both the environments have separate databases. During development base url of development environment must be used.

Defining internal objects
-------------------------

	================= ============================================
	 USER ROLES   	    RESPONSE
	================= ============================================ 
	 **Employee**  				* We define *employee* as an entity which is being verified on the platform.
	 		             	
	 **User** 						* We define *user* as an company admin.

	 **Company**					* We define *company* as an entity having group of *users* (Admins).

	 **Super Admin**			* We define *Super Admin* as springVerify-US team.
	================= ============================================

Environment Urls
----------------

	**Development** : https://api-stage.us.springverify.com

	**Production** : https://api.us.springverify.com

.. note::
	 Currently we have two types of coupons (open/closed). Please contact springVerify-US team regarding coupon code for your company.