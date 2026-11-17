# Eval questions

The questions we ask an assistant to check the GONDI MCP server's answers. Run
them yourself to reproduce an answer, or to check a fix.

## How to run

1. Connect only the GONDI server (`https://api.gondi.xyz/mcp`) to your
   assistant, so every answer comes from GONDI data. With Claude Code:

   ```sh
   claude mcp add --transport http gondi https://api.gondi.xyz/mcp
   claude -p "What's the top bid on Pudgy Penguins on GONDI?"
   ```

2. Ask each question in a new conversation.
3. For questions 15 to 17, replace `<your wallet>` with an address, GONDI
   username or ENS name. Questions 15 and 16 need a wallet with open loans as a
   borrower; question 17 needs a wallet that holds NFTs not already in a loan.
4. Check the answer against the same page on [gondi.xyz](https://www.gondi.xyz).
   Answers are live data, so numbers change between runs.

## Questions

The tools listed are the ones a good answer calls.

| # | Question | Expected tools |
| --- | --- | --- |
| 01 | Give me an overview of the GONDI lending market right now. | `get_lending_market_overview` |
| 02 | What collections are the best to lend against on GONDI? | `list_lending_opportunities` |
| 03 | What APR can I earn lending against Pudgy Penguins on GONDI? | `search_gondi`, `get_collection_lending_stats` |
| 04 | Show me the highest-APR loan offers on GONDI right now. | `list_loan_offers` |
| 05 | Who are the top lenders on GONDI? | `get_top_lenders` |
| 06 | How does lending work on GONDI? | `get_lending_guide` |
| 07 | Which chains and currencies can I lend in on GONDI? | `get_supported_markets` |
| 08 | How much has been lent against CryptoPunks on GONDI? | `search_gondi`, `get_collection_lending_stats` |
| 09 | I want to lend 10 WETH on GONDI. What terms should I offer on Milady Maker to get filled? | `search_gondi`, `get_collection_lending_stats`, `list_loan_offers` |
| 10 | What's the top bid on Pudgy Penguins on GONDI? | `search_gondi`, `get_collection_market` |
| 11 | What's the top bid on CryptoPunk #1234 on GONDI? | `search_gondi`, `get_nft_market` |
| 12 | What's the cheapest Fidenza for sale on GONDI? | `search_gondi`, `list_cheapest_for_sale` |
| 13 | What's the cheapest NFT for sale by XCOPY on GONDI? | `search_gondi`, `list_cheapest_for_sale` |
| 14 | What are mishark's lending returns on GONDI? | `get_lender_returns` |
| 15 | I'm wallet `<your wallet>`. Which of my GONDI loans expire in the next 7 days? | `list_borrower_loans` |
| 16 | I'm wallet `<your wallet>`. Which of my GONDI loans should I renegotiate? | `list_borrower_loans` |
| 17 | I'm wallet `<your wallet>`. What good borrowing opportunities do I have on GONDI? | `list_borrowing_opportunities` |
| 18 | What's the cheapest loan I can get against a Pudgy Penguin on GONDI? | `search_gondi`, `list_loan_offers` |

Found a wrong answer? [Open an issue](https://github.com/gondixyz/mcp/issues/new/choose)
with the question number and what the assistant said.
