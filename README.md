# prrpssautobot
A fun & friendly group fund management service

login > 
    login_check_do > set globals / session vars
index
  > redirect users to >> account_view
  > admin home page [NOW, ALL USERS]
      > title
      > walletBar();
      > beautyBox();
      > fundSummaryTable(); [ALL users LOOP, UP TO NOW]
      > fundActivity
      > glossary();
privacy > user_preferences
user_profile
    > user_profile_edit
account_view
    > title
    > walletBar();
    > summary();
    > glossary();
    > accountSummaryTable(); [user (1), ]
    > accountActivity(); 
    
    
 //////////////////////////////////////////////////////////////////
 //////////////////////////////////////////////////////////////////
 //////////////////////////////////////////////////////////////////
 
 FUNCTIONS
 
 require ('functions_loader.php');
 
 function_loader.php:
 
      1. > ledger_read_functions
      2. > ledger_action_functions
      3. > percentage_functions
      4. > conversion_functions
      5. > global functions
      6. > user_functions
      7. > administration functions
      8. > page_functions
      9. > table_functions
                  
  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
  
1. > ledger_read_functions
      1. TNX_ID = NULL;
      2. user_TNX_ID = NULL;
      3. user_ID = NULL;
      4. trasactTime = NULL;
      5. > type  
            > deposits
            > withdrawals
            > balance
            > fees
            > transfers
            > payments
      6. > view_type
            > one
            > all
    
  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
2. > ledger_action_functions
      1. > deposit_add
      2. > withdrawal_add
      3. > tranfers
      
  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
3. > percentage_functions

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
4. > conversion_functions

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
5. > global functions

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
6. > user_functions
    1. userBar
    2. userList() -- user_level >= 9
    3. totalUsers
    4. getUser()

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
7. > administration functions
    1. review pending tasks
    2. import_data();
    3. split_payment

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
8. > page_functions
      1. page_head();
          > start_page_load_time();
      2. page_foot();
      3. 

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
9. > table_functions 

  // // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
10. > system_functions 
      1. register >> submit_registration >> email
      2. log_in >> log_in_check_do
