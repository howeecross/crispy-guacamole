# vision.com. 
TECHSMART INTERNATIONAL
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <meta name="viewport" content="width=640">

    <link rel="stylesheet" href="stylesheets/core.css" media="screen">
    <link rel="stylesheet" href="stylesheets/mobile.css" media="handheld, only screen and (max-device-width:640px)">
    <link rel="stylesheet" href="stylesheets/github-light.css">

    <script type="text/javascript" src="javascripts/modernizr.js"></script>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
    <script type="text/javascript" src="javascripts/headsmart.min.js"></script>
    <script type="text/javascript">
      $(document).ready(function () {
        $('#main_content').headsmart()
      })
    </script>
    <title>Vision.com by howeecross</title>
  </head>

  <body>
    <a id="forkme_banner" href="https://github.com/howeecross/vision.com">View on GitHub</a>
    <div class="shell">

      <header>
        <span class="ribbon-outer">
          <span class="ribbon-inner">
            <h1>Vision.com</h1>
            <h2></h2>
          </span>
          <span class="left-tail"></span>
          <span class="right-tail"></span>
        </span>
      </header>

      <section id="downloads">
        <span class="inner">
          <a href="https://github.com/howeecross/vision.com/zipball/master" class="zip"><em>download</em> .ZIP</a><a href="https://github.com/howeecross/vision.com/tarball/master" class="tgz"><em>download</em> .TGZ</a>
        </span>
      </section>


      <span class="banner-fix"></span>


      <section id="main_content">
        <h3>
<a id="welcome-to-github-pages" class="anchor" href="#welcome-to-github-pages" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Welcome to GitHub Pages.</h3>
<p>This automatic page generator is the easiest way to create beautiful pages for all of your projects. Author your page content here <a href="https://guides.github.com/features/mastering-markdown/">using GitHub Flavored Markdown</a>, select a template crafted by a designer, and publish. After your page is generated, you can check out the new <code>gh-pages</code> branch locally. If you’re using GitHub Desktop, simply sync your repository and you’ll see the new branch.</p>
<h3>
<a id="designer-templates" class="anchor" href="#designer-templates" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Designer Templates</h3>
<p>We’ve crafted some handsome templates for you to use. Go ahead and click 'Continue to layouts' to browse through them. You can easily go back to edit your page before publishing. After publishing your page, you can revisit the page generator and switch to another theme. Your Page content will be preserved.</p>
<h3>
<a id="creating-pages-manually" class="anchor" href="#creating-pages-manually" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Creating pages manually</h3>
<p>If you prefer to not use the automatic generator, push a branch named <code>gh-pages</code> to your repository to create a page manually. In addition to supporting regular HTML content, GitHub Pages support Jekyll, a simple, blog aware static site generator. Jekyll makes it easy to create site-wide headers and footers without having to copy them across every page. It also offers intelligent blog support and other advanced templating features.</p>
<h3>
<a id="authors-and-contributors" class="anchor" href="#authors-and-contributors" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Authors and Contributors</h3>
<p>You can @mention a GitHub username to generate a link to their profile. The resulting <code>&lt;a&gt;</code> element will link to the contributor’s GitHub Profile. For example: In 2007, Chris Wanstrath (<a class="user-mention" data-hovercard-type="user" data-hovercard-url="/users/defunkt/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/defunkt">@defunkt</a>), PJ Hyett (<a class="user-mention" data-hovercard-type="user" data-hovercard-url="/users/pjhyett/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/pjhyett">@pjhyett</a>), and Tom Preston-Werner (<a class="user-mention" data-hovercard-type="user" data-hovercard-url="/users/mojombo/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/mojombo">@mojombo</a>) founded GitHub.</p>
<h3>
<a id="support-or-contact" class="anchor" href="#support-or-contact" aria-hidden="true"><span aria-hidden="true" class="octicon octicon-link"></span></a>Support or Contact</h3>
<p>Having trouble with Pages? Check out our <a href="https://help.github.com/pages">documentation</a> or <a href="https://github.com/contact">contact support</a> and we’ll help you sort it out.</p>
      </section>

      <footer>
        <span class="ribbon-outer">
          <span class="ribbon-inner">
            <p>this project by <a href="https://github.com/howeecross">howeecross</a> can be found on <a href="https://github.com/howeecross/vision.com">GitHub</a></p>
          </span>
          <span class="left-tail"></span>
          <span class="right-tail"></span>
        </span>
        <p>Generated with <a href="https://pages.github.com">GitHub Pages</a> using Merlot</p>
        <span class="octocat"></span>
      </footer>

    </div>

    
  </body>
</html>
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract PredictionMarket {
    address public owner;
    string public marketDescription;
    uint256 public outcomePrice;
    bool public marketResolved;
    address public winningOutcome;
    address[] public participants;

    enum MarketOutcome { Yes, No }

    // Mapping to keep track of balances for participants
    mapping(address => uint256) public balances;

    // Event for recording predictions
    event Prediction(address indexed participant, MarketOutcome outcome, uint256 amount);

    constructor(string memory description) {
        owner = msg.sender;
        marketDescription = description;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    function makePrediction(MarketOutcome outcome) public payable {
        require(!marketResolved, "Market is already resolved");
        require(outcomePrice > 0, "Outcome price must be set first");
        require(msg.value > 0, "Prediction amount must be greater than 0");

        // Record the prediction
        emit Prediction(msg.sender, outcome, msg.value);

        // Update the balance of the participant
        balances[msg.sender] += msg.value;
        participants.push(msg.sender);
    }

    function resolveMarket(MarketOutcome _winningOutcome, uint256 _outcomePrice) public onlyOwner {
        require(!marketResolved, "Market is already resolved");

        marketResolved = true;
        winningOutcome = msg.sender;
        outcomePrice = _outcomePrice;

        // Distribute funds to participants based on the outcome
        for (uint256 i = 0; i < participants.length; i++) {
            address participant = participants[i];
            if (_winningOutcome == MarketOutcome.Yes) {
                // Pay participants who predicted 'Yes' for the winning outcome
                if (balances[participant] > 0) {
                    payable(participant).transfer(balances[participant] * outcomePrice);
                }
            } else {
                // Pay participants who predicted 'No' for the winning outcome
                if (balances[participant] > 0) {
                    payable(participant).transfer(balances[participant]);
                }
            }
        }
    }

    function withdraw() public {
        require(marketResolved, "Market is not resolved yet");
        require(balances[msg.sender] > 0, "No balance to withdraw");

        uint256 balanceToWithdraw = balances[msg.sender];
        balances[msg.sender] = 0;
        payable(msg.sender).transfer(balanceToWithdraw);
    }
}






