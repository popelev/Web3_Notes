https://www.chaijs.com/api/bdd/

https://www.npmjs.com/package/solidity-coverage

```javascript
const { expect, assert } = require("chai")


describe("constructor", async function () {
	it("sets the aggregator addresses correctly", async function () {
		const response = await fundMe.s_priceFeed()
		assert.equal(response, mockV3Aggregator.address)
	})
})
describe("fund", async function () {
	it("sFails if you dont send enough ETH", async function () {
		await expect(fundMe.fund()).to.be.revertedWith(
			"You need to spend more ETH!"
		)
	})
})

```