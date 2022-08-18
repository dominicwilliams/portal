# Launching the SNS in production
This describes the detailed steps for launching an SNS.

:::caution

In the following steps we assume that you have already
[collected the developer and airdrop principals](predeployment.md/#principals)
and [chosen the initial SNS parameters in a .yaml file](predeployment.md).
We assume that you control one principal `identityDevNeuron` that owns a developer neuron.
Moreover, you control one principal `identityDevDeploy` that is a `dfx` identity and 
a controller of the dapp canister(s) that you would like to hand over to the SNS.
We also recommend that before following these steps and launching an
SNS in production, you have [tested the SNS](local-testing.md).

:::


You are now ready to launch your SNS. There are some steps that are needed
for launching an SNS and some steps that we recommend as intermediate
sanity checks that up to this point everything works fine.
We mark the latter by the keyword _(Recommended)_.

#### 1. Get your two principals ready

<!--
Open terminal with dfx, ready for commands when we say "use `dfx` identity 
`identityDevDeploy`.
For things to do with `sns-quill` will say "use `sns-quill` principal `identityDevNeuron`"
We recommend that message are signed on air-gapped computer and sent to IC
on connected computer. 
To follow this recommendation, should open one terminal on
air-gapped computer and have another one to just forward sns-quill commands
on the connected computer.
-->

#### 1. Make a call to the SNS wasm modules canister on the NNS subnet.
To make this call, use your `dfx` identity `identityDevDeploy` and 
the command as described 'here'. <!--TODO-CLI/dfx-Link: -->
Upon receiving this call, the SNS wasm modules canister will deploy
an SNS with your chosen initial parameters.

#### 2. Add the SNS root canister as a controller to your dapp canister(s).
To do so, use your `dfx` identity `identityDevDeploy` and 
the command described 'here'.
<!-- TODO: add this to CLI/dfx tool as need to learn SNS canisters -->


#### 3. _(Recommended)_ Test upgrading the dapp canister(s) by SNS proposal. _{#step3}_
To test this, make an SNS proposal to upgrade one of the dapp canisters to
a new wasm version.
Then, ensure that sufficiently many initial neurons (developer and airdrop
neurons) vote on the proposal so that it is adopted.
Then, confirm that the proposal has been executed by checking that the dapp has been
upgraded. 

To submit an SNS proposal, use your `sns-quill` principal `identityDevNeuron`
and learn what command to use [here](https://github.com/dfinity/sns-quill#submit-a-proposal) 
<!-- TODO: SNS quill documentation to make proposal and link to it-->.
To vote on an SNS proposal use your `sns-quill` principal `identityDevNeuron`
and use the command explained [here](https://github.com/dfinity/sns-quill#vote-on-a-proposal).

:::info

This step is one of the reasons why you should ensure that you can
reach and get buy in from a majority of the inital neurons. Also, as we 
recommend `sns-quill` principals for initial neurons and as they have to be
able to vote on SNS proposals even before the decentralization sales,
this requires that initial neuron holders are comfortable
using a command line tool for voting.

:::

#### 4. Remove all other controllers from the dapp canister(s)
Once you convinced yourself that the dapp canister(s) can be upgraded by
the SNS, you should remove yourself, as well as any other developers,
from the list of controllers that the dapp canister(s) have.
Note that without this, the next step will fail.

To do this, use your `dfx` identity `identityDevDeploy` and the command 'here'
where you specify as the principals to be removed all existing controller principals
except for the SNS root that you have already added.
<!--TODO-CLI/dfx-Link: should already exist in DFX -->

#### 5. Register the dapp to the SNS
Next, you will register the dapp canisters that are now controlled by the SNS
in the SNS root canister. This is to make sure that the SNS root canister
is aware of the canisters that it officially governs. 
This ensures, for example, that if you request a canister summary from the
SNS root canister, then the dapp canisters are included in this summary and 
you can learn how many cycles they still have and other information.

Registering a dapp under an SNS is again done by an SNS proposal.
To make such a proposal and vote on it, use your `sns-quill` principal
`identityDevNeuron` and follow the instructions from [Step 3](#step3).
As in [Step 3](#step3),
sufficient initial neurons have to vote to reach a majority.

<!-- TODO:IN CASE THIS IS NEEDED: Repeat these steps for all canisters that you would like to register.-->

#### 6. _(Recommended)_ Test upgrading the dapp canister(s) by SNS proposal.
To make sure that you can still upgrade the dapp canister(s) by SNS proposal,
you can repeat [Step 3](#step3) at this point.

#### 7. Submit an NNS proposal to start the decentralization sale.
At this point, you have handed over the control of your dapp to the Internet
Computer. 
For the Internet Computer to start the SNS decentralization sale,
anyone with an eligible NNS neuron can now submit an NNS proposal
that asks the NNS community to do so.
To submit an NNS proposal with dfx, they can use the following command
```
<!--TODO-code: --> 
```
Note that anyone can send such a proposal, but as the original developer
of the dapp you probably want to make sure that such a proposal is submitted.

#### 8. Wait for the NNS to approve the sale & continue evolving the dapp! 
After the last step, there is nothing more to do for you as the original dapp 
developer to launch the SNS!
The dapp has been handed over to the IC and the NNS is voting on whether 
the SNS should be launched.
If the proposal is adopted, the SNS decentralization sale will be 
started with the configurations that you have defined in the
[initialization file](predeployment.md).
If the proposal is rejected, the dapp canisters' controllers are automatically set
back to the developer principals that you
have defined in the [initialization file](predeployment.md).

During this time you can further upgrade your dapp canister(s), for
example to add new features or fix bugs, by sending additional
SNS proposals as explained in
[Step 3](#step3).
You can also add new dapp canisters under the SNS control, if required, 
by additional SNS proposals as explained in
[Step 5](#5-register-the-dapp-to-the-sns).
Finally, if there are new blessed deployments of the SNS canisters, you can
upgrade the SNS canisters by an SNS proposal. 
To make such a proposal, you can use the following dfx command:
``` 
<!--TODO-code: --> 
```  
