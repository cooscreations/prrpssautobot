# prrpssautobot
A fun & friendly group fund management service.
Please note that in some cases, the 'notes' variable is being used to drive logic, as it is expected that the notes are automatically populated, based on the user activity, for example making a transfer between users. Note, however, if users are allowed to change these vars, certain functionality may fail. :-O

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // //    GLOSSARY / TERMS  // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
 
 1. TNX             = Activity provided (ideally via REST) from public trading exchange
 2. historical_data = Bitcoin to USD price data from CoinMarketCap (can this also be taken from the exchange?)
 3. user_TNX        = Manually entered by fundManger, later to become user-generated requests, plus split profits


_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // //  SUPER VARS // // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
 
Note: These are hard-coded SQL "SELECT ('here')" accoring to table design, 
for quick way to combine access to certain table views:
 
 1. TNX
 2. user_TNX
 3. historical_data
 4. TNX + user_TNX
 5. TNX + user_TNX + historical_data

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // //   ALL PAGES    // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
 
1. Get page data from DB
2. Check they are OK to view this page (`pages` auth_level > $_SESSION['user_level'])
    a. if not logged in >> login
    b. else > show error message
3. Check super vars:
    1. user_ID
    2. TNX_ID
    3. user_TNX_ID (get, or set to $_SESSION['user_ID'])
    4. transactTime (get, or set to now)
        a. when set, set rate: rate(transactTime) - (((hi-lo)/2) + lo)
 4. set totalUsers(transactTime) (lower priority 'fun' info)
 5. page_head();
 
 /* PAGE CONTENT GOES HERE! */
 
 6. page_foot();
 
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // //  INDIVIDUAL PAGES // // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

1. login > 
  1. login_check_do > set globals / session vars
2. index
  1. redirect users to >> account_view
  2. admin home page [NOW, ALL USERS]
    1. title
    2. walletBar();
    3. beautyBox();
    4. fundSummaryTable(); [ALL users LOOP, UP TO NOW]
    5. fundActivityTable();
    6. glossary();
3. privacy (static HTML page)
  1. >> user_preferences
4. user_profile
  1. user_profile_edit
5. account_view
  1. title
  2. walletBar(); (include withdrawal amounts / cash fee etc.)
  3. beautyBox();
  4. summary();
  4. glossary();
  5. accountSummaryTable(); [user (1), ]
  6. accountActivity(); 
6. historical_data (table)
7. exchange_data (table: TNX & BTC rate (+ count user_TNX?))
8. accountActivity_data (advanced_table - user_TNX + (exhcange & rate) JOINED)
9. TNX_view()
10. transfer_view(user_TNX_ID)? 
11. BTC_rate_view(transactTime)
12. deposit
13. withdraw
14. transfer
 
 _________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // //// // // // // // // // //     FUNCTIONS   // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
 
 require ('functions_loader.php');
 
 function_loader.php: // LOAD ALL THE FILES LISTED BELOW:
 
1. ledger_read_functions
2. ledger_action_functions
3. percentage_functions
4. conversion_functions
5. global functions
6. user_functions
7. administration functions
8. page_functions
9. table_functions

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
  
1. ledger_read_functions (...
  1. TNX_ID         = NULL;
  2. user_TNX_ID    = NULL;
  3. user_ID        = NULL;
  4. transactTime   = NULL;
  5. type  
    1. deposits
    2. withdrawals
    3. balance >> NOW + PREVIOUS (ballance +/- amount)
    4. fees
    5. transfers in
    6. transfers out
    7. payments in
    8. payments out
  6. view_type
    1. one
    2. all - true
    3. all - after royalties
    4. all - after withdrawalFee
    
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

2. ledger_action_functions
  1. deposit
    1. _add
    2. _edit / _approve
    3. _delete
 2. withdrawal_add
 3. tranfers 
     
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

3. percentage_functions
    1. if (+) - growth_percentage
    2. if (-) - loss_percentage

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

4. conversion_functions
  1. sat_to_USD(sat, transactTime);
  2. rate(transactTime); // CHANGE TO rate_BTC_to_USD ??? <<< send sats?
  3. AUM_share( userBalance / walletBalance )
  4. withdrawalFee() -- return: original_amount, end_amount, fee_amount
  5. operatingFee() -- return: original_amount, end_amount, fee_amount
  6. display_satoshi(sat_qty)
    0. no format, number only in XBT
    1. .= labels: 
      1. XBT
      2. TOTAL
      3. FEES
     4. % <<<<<<<< ? can we do this ?
   2. .= add 'fab fa-bitcoin text-warning' icon
   3. .= add USD amount & rate   (?? - should we be firing another function here?)
   4. .= red / green / muted number for regular numbers
   5. .= red for fees (positive number is converted to negative for red and '-' symbol in front)
   6. .= icon for growth direction (+ = up, - = down) - icon up-arrow / down-arrow

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

5. global functions

    
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

6. user_functions
 1. userBar($_SESSION['user_ID']) -- 
   1. image
   2. name
   3. user_level
   4. Profile
   5. Wallet > modal popUp
   6. ledger_action_buttons(); (balance, deposits, withdrawals, transfers, PNL)
   6. Logout
   7. where $_SESSION['user_level'] > 9 - 
 2. userList() -- if ($_SESSION['user_level'] >= 9)
 3. totalUsers() -- (today, week, month, year, all_time)
 4. getUser(user_ID)
 5. walletBar -- include SPLIT button
    
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // //  
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

7. administration functions
  1. show_pending_tasks() // get any records in ANY table where (record_status == 1)
  2. import_data();
  3. split_payment
      a. splits_to_VIPs = ((walletBalance - prevBalance) / 2 )
      b. user AUM share = userBalance / balance
      c. ((user AUM share * splits_to_VIPs) * withdrawalFee)
      d. save splits & fees to DB
  4. backup_site
  5. import_TNX
  6. import_historical_data
    
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // //  
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

8. page_functions
      0. first things first:
          a. get_page_info($filename)
          b. check_auth($page_ID, $user_level)
          b. get / set vars? 
             * ARRAY( TNX_ID, user_TNX_ID, user_ID, transactTime, BTC_rate, numUsers)
      1. page_head();
          a. start_page_load_time();
          b. page_title();
          c. open page content section
      2. page_foot();
          a. close page content section
      3. main_menu();
      4. beautyBox();
      5. big_info_box();
      6. wide_info_box();
      7. type_and_status_bar();
      8. snapshot_select(); // catch vars - user_ID, TNX_ID, user_TNX_ID, transactTime
    
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // //  
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

9. table_functions 
      1. exchange_data
         a. (with associated user_TNX in advanced table) - add BTC_rate
         b. (table: TNX & BTC rate (+ count user_TNX?))
      2. user_TNX data (JOIN exchange_data)
      3. accountSummaryTable(); [user (1), ]
      4. accountActivity(); 
      5. historical_data (table)
      6. exchange_data (table: TNX & BTC rate (+ count user_TNX?))
      7. accountActivity_data (advanced_table - user_TNX + (exhcange & rate) JOINED)
      8. fundSummaryTable(); [ALL users LOOP, UP TO NOW]
      9. fundActivityTable();
      10. glossary();
      

_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

10. system_functions 
      1. register >> submit_registration >> email
      2. log_in >> log_in_check_do
      3. reset_pwd
      4. send_SMS()
      5. SMS_auth()
         a. create code
         b. send
         c. write to DB
         d. confirm: enter code, compare, update DB
      
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////
_________________________________________________________________________________________
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // //
// // // // // // // // // // // // // // // // // // // // // // // // // // // // // // 
_________________________________________________________________________________________

EXAMPLES
_________________________________________________________________________________________

Examples of using the functions:

1. getBalance(); // return exchange balance where transactTime == transactTime
