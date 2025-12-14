export async function checkStatusAndPerformAction() {
  const status = await getCurrentStatus();
  console.log(`Current Status is: ${status}`);

  // ✅ STATUS = ACTIVE → Related Policies → Open Context
  if (status === 'Active') {
    console.log('Status is Active. Opening Related Policies.');

    await click(relatedPolicies());
    await click(openContext());

    return;
  }

  // ❌ STATUS = SUBMISSION → Decline Quote
  if (status === 'Submission') {
    console.log('Status is Submission. Declining quote.');

    await clickStatusTransition();
    await clickChangeStatus();
    await selectDeclinedOption();
    await fillDeclined('Declined');
    await clickChangeStatusReason();
    await fillStatusReason('No Coverage Requested');
    await clickSave();

    return;
  }

  console.log('Status is neither Active nor Submission. No action taken.');
}
