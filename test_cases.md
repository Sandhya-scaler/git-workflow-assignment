# Login System – Test Case Design

## Functional Requirements

| ID | Requirement |
|----|-------------|
| FR1 | Username must not be empty. |
| FR2 | Password must be at least 6 characters long. |
| FR3 | Valid username and password should allow successful login. |
| FR4 | Invalid credentials should display an error message. |
| FR5 | After 3 consecutive failed login attempts, the account should be locked. |

## Test Data

- Valid registered account: Username `user1`, Password `pass123` (7 characters).
- Unless a test case says otherwise, each test starts with account `user1` unlocked and with 0 failed login attempts.

## Test Cases

| Test Case ID | Test Scenario | Input Data | Expected Output |
|---|---|---|---|
| TC01 | Successful login with valid credentials (FR3) | Username: user1; Password: pass123; Click Login | Login succeeds. Message "Login successful." is displayed and the user is taken to the home page. Failed-attempt count for user1 stays at 0. |
| TC02 | Empty username (FR1) | Username: (empty); Password: pass123; Click Login | Login is blocked. Message "Username is required." is displayed. Credentials are not checked and the failed-attempt count is not increased. |
| TC03 | Empty password (FR2) | Username: user1; Password: (empty); Click Login | Login is blocked. Message "Password must contain at least 6 characters." is displayed. Credentials are not checked and the failed-attempt count is not increased. |
| TC04 | Password shorter than 6 characters – boundary value 5 (FR2) | Username: user1; Password: pass1 (5 characters); Click Login | Login is blocked. Message "Password must contain at least 6 characters." is displayed. Credentials are not checked and the failed-attempt count is not increased. |
| TC05 | Password exactly 6 characters – boundary value 6 (FR2, FR4) | Username: user1; Password: pass12 (6 characters); Click Login | The length rule is satisfied, so "Password must contain at least 6 characters." is NOT displayed. Credentials are checked; because pass12 is not the correct password, "Invalid username or password." is displayed. Failed-attempt count for user1 becomes 1. |
| TC06 | Invalid username (FR4) | Username: user9 (not registered); Password: pass123; Click Login | Login fails. Message "Invalid username or password." is displayed. The account user1 is not affected. |
| TC07 | Invalid password for a valid username (FR4) | Username: user1; Password: wrong123; Click Login | Login fails. Message "Invalid username or password." is displayed. Failed-attempt count for user1 becomes 1; account is not locked. |
| TC08 | Three consecutive failed login attempts lock the account (FR4, FR5) | Username: user1; Attempt 1 Password: wrong123; Attempt 2 Password: wrong456; Attempt 3 Password: wrong789 | Attempts 1 and 2: "Invalid username or password." is displayed. Attempt 3: "Account locked after 3 consecutive failed login attempts." is displayed and account user1 is locked. |
| TC09 | Login attempt with correct credentials after the account is locked (FR5) | Precondition: user1 locked by TC08. Username: user1; Password: pass123; Click Login | Login is denied even though the credentials are correct. Message "Account locked after 3 consecutive failed login attempts." is displayed and the account stays locked. |
| TC10 | Failed attempts that are not consecutive do not lock the account (FR3, FR5) | Username: user1; Attempt 1: wrong123; Attempt 2: wrong456; Attempt 3: pass123; Attempt 4: wrong789 | Attempts 1 and 2: "Invalid username or password." Attempt 3: "Login successful." and the failed-attempt count resets to 0. Attempt 4: "Invalid username or password." with count 1. The account is NOT locked. |

## Requirement Coverage

| Requirement | Covered by |
|---|---|
| FR1 – Username must not be empty | TC02 |
| FR2 – Password at least 6 characters | TC03, TC04, TC05 |
| FR3 – Valid credentials allow login | TC01, TC10 |
| FR4 – Invalid credentials show an error | TC05, TC06, TC07, TC08 |
| FR5 – Lock after 3 consecutive failures | TC08, TC09, TC10 |

## Assumptions

The assignment does not define every detail of the system's behaviour. These test cases assume the following:

1. Exact message texts: "Username is required.", "Password must contain at least 6 characters.", "Invalid username or password." and "Account locked after 3 consecutive failed login attempts." (taken from the assignment's examples). The success message "Login successful." and redirection to the home page are assumed.
2. An empty password is treated as shorter than 6 characters, so it shows the minimum-length message (no separate "Password is required." message is specified).
3. Input validation (empty username, short password) happens before the credentials are checked, and these validation errors do not count as failed login attempts.
4. Failed attempts are counted per username. A successful login resets the count to 0, which is why only consecutive failures lock the account.
5. The third consecutive failed attempt itself displays the lock message.
6. A locked account stays locked for all later attempts, including with the correct password. No unlock process (time-out or administrator reset) is specified, so it is not tested.
7. The same general message "Invalid username or password." is shown for both an unknown username and a wrong password, so the system does not reveal which one was wrong.
