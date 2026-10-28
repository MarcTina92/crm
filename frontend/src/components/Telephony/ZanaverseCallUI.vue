<template>
  <div></div>
</template>
<script setup>
// Zanaverse calling: the call is placed server-side (CRM -> zanaverse_telephony ->
// telephony middleware -> FreeSWITCH), which rings the agent's own phone first.
// The "zanaverse" integration flag comes from zanaverse_telephony's override of
// crm.integrations.api.is_call_integration_enabled (agents with an extension only).
import { call, toast } from 'frappe-ui'

function setup() {}

async function makeOutgoingCall(number) {
  try {
    const result = await call(
      'zanaverse_telephony.zanaverse_telephony.api.make_call',
      { destination_number: number },
    )
    if (result?.status === 'accepted') {
      toast.success(__('Call initiated - your phone should ring shortly'))
    } else {
      toast.error(__('Call could not be started'))
    }
  } catch (e) {
    toast.error(__('Call failed: {0}', [e.messages?.[0] || e.message]))
  }
}

defineExpose({ makeOutgoingCall, setup })
</script>
