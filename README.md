# GONDI MCP Server

[GONDI](https://www.gondi.xyz) is an NFT marketplace with peer-to-peer lending
built in. On Ethereum, Hyperliquid (HyperEVM) and Robinhood Chain you buy, sell,
list and bid on NFTs, and borrow or lend against them.

The GONDI MCP server gives AI assistants live GONDI data through the
[Model Context Protocol](https://modelcontextprotocol.io): floor prices, top bids
and the cheapest listings, loan offers and lending rates, a borrower's open loans
and what their NFTs can borrow, top lenders and their returns. Add it to your
assistant once, then ask questions in plain language.

This repository describes the hosted server and is the place to report problems
with it. The server itself is hosted by GONDI; there is nothing to install or run.

## Endpoint

| | |
| --- | --- |
| URL | `https://api.gondi.xyz/mcp` |
| Transport | Streamable HTTP |
| Authentication | None: no API key, no wallet |
| Access | Read-only: it never signs or sends transactions |
| MCP Registry | `xyz.gondi/mcp` |

## Setup

### Claude (claude.ai and Claude Desktop)

1. Open **Customize → Connectors**.
2. Select **+ Add**, then **Add custom connector**.
3. Name it `GONDI`, enter `https://api.gondi.xyz/mcp` as the URL and select
   **Continue**.
4. Choose **No sign in** under Authentication, then **Add**.
5. In a chat, turn it on from the **+** button → **Connectors**.

On Team and Enterprise plans an owner adds the connector under
**Organization settings → Connectors** first.

### Claude Code

```sh
claude mcp add --transport http gondi https://api.gondi.xyz/mcp
```

### Cursor

Add the server to `~/.cursor/mcp.json` (every project) or `.cursor/mcp.json`
(one project):

```json
{
  "mcpServers": {
    "gondi": {
      "url": "https://api.gondi.xyz/mcp"
    }
  }
}
```

### VS Code

Run **MCP: Add Server** from the Command Palette, choose **HTTP** and enter
`https://api.gondi.xyz/mcp`. Or add it to `.vscode/mcp.json` in your workspace:

```json
{
  "servers": {
    "gondi": {
      "type": "http",
      "url": "https://api.gondi.xyz/mcp"
    }
  }
}
```

### ChatGPT

In ChatGPT on the web, open **Plugins**, select **+** and **Add custom MCP
server**. Enter `https://api.gondi.xyz/mcp` as the server URL, choose **No
authentication**, accept the warning and create it, then install it before using
it in a chat. Custom MCP servers depend on your ChatGPT plan and, in a workspace,
on what its admins allow.

### Other clients

Any client that supports remote MCP servers over streamable HTTP works: point it
at `https://api.gondi.xyz/mcp` with no authentication.

## Tools

The server offers 14 tools. Tools that take a slug get it from `search_gondi`
rather than guessing it. Wallet tools take an address, a GONDI username or an ENS
name. Chains are `ethereum`, `hyperliquid` and `robinhood`; currencies are
`WETH`, `USDC`, `WHYPE`, `HUSDC`, `RWETH` and `USDG`. Your client shows each
tool's full description and input schema.

### Search

| Tool | What it answers |
| --- | --- |
| `search_gondi` | Collections, artists, NFTs and users matching a name, each with its slug or address, chain and GONDI page. |

### Market

| Tool | What it answers |
| --- | --- |
| `get_collection_market` | A collection's floor price and top collection bid, in its currency and in USD. |
| `get_nft_market` | One NFT's listing price, top bid, last sale, estimated value, and whether it is collateral in an open loan. |
| `list_cheapest_for_sale` | The cheapest NFTs for sale from a collection or by an artist, with the page to buy each. |

### Lending

| Tool | What it answers |
| --- | --- |
| `get_lending_market_overview` | The whole lending market in USD: open principal, interest paid and accruing, volume and APRs of new loans. |
| `get_supported_markets` | The chains and currencies GONDI lends in, the loan contract on each chain, accepted durations and the protocol fee. |
| `get_lending_guide` | How lending on GONDI works, step by step, with its risks. |
| `list_lending_opportunities` | Collections borrowers take loans against, ranked by open principal, loans, offers or volume. |
| `get_collection_lending_stats` | One collection's lending picture: open loans, repayment rate, principal lent, and the ranges of its loan terms. |
| `list_loan_offers` | Active loan offers: principal, APR and duration; sorted by lowest APR, the cheapest loan a borrower can take. |
| `get_top_lenders` | The leading lenders over 30, 90 or 180 days, with open principal, earnings, net APR and default rate. |
| `get_lender_returns` | One lender's record per currency: principal lent, net profit, net APR, defaults and open positions. |

### Borrowing

| Tool | What it answers |
| --- | --- |
| `list_borrower_loans` | A borrower's open loans: due dates, repayment due, and the renegotiation or market offers that could make them cheaper. |
| `list_borrowing_opportunities` | The NFTs a wallet holds that are not in a loan, each with the largest loan offer it can take today. |

## Example questions

- "What's the floor and the top bid on Pudgy Penguins?"
- "What's the top bid on Milady #1234, and what is it listed for?"
- "Show me the five cheapest CryptoPunks for sale."
- "What's the cheapest piece for sale by XCOPY?"
- "Which of my loans expire this week?" (give your address, GONDI username or
  ENS name)
- "Which of my loans should I renegotiate?"
- "What can I borrow against the NFTs in vitalik.eth?"
- "What's the cheapest loan I can take on Azuki?"
- "What APR can I earn lending against Pudgy Penguins?"
- "Which collections are best to lend against?"
- "Who are the top lenders this month and what do they earn?"
- "What are mishark's lending returns?"
- "How does GONDI lending work and what are the risks?"

[`eval/questions.md`](eval/questions.md) has the list we check answers against,
so you can reproduce them.

## Limits

- **Read-only.** No tool holds keys, signs or sends transactions. Answers link
  to the page on [gondi.xyz](https://www.gondi.xyz) where you act with your own
  wallet: buy, bid, borrow, repay, renegotiate or refinance.
- **No API key.** Requests are rate-limited per IP address.
- **Live data.** Answers come from GONDI's public data at the moment of the
  call.
- **Not financial advice.** Suggestions are based on market data only. Buying,
  borrowing and lending against NFTs carry risk: a loan not repaid by its expiry
  defaults and the borrower loses the NFT.

## Report a wrong answer

If an assistant gives a wrong answer from GONDI data, or you need a tool that
doesn't exist yet, [open an issue](https://github.com/gondixyz/mcp/issues/new/choose). Include the question
you asked and what the assistant answered.

## Links

- Documentation: <https://docs.gondi.xyz/gondi-v3/mcp-server/>
- MCP Registry entry: <https://registry.modelcontextprotocol.io/v0/servers?search=xyz.gondi>
- Server card: <https://www.gondi.xyz/.well-known/mcp/server-card.json>
- GONDI: <https://www.gondi.xyz>

## License

[MIT](LICENSE)
