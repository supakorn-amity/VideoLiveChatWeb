<template>
  <b-dropdown
    id="MessageOptions"
    aria-role="list"
    :position="lastChild ? 'is-top-left' : 'is-bottom-left'"
    :mobile-modal="false"
  >
    <template #trigger>
      <b-icon icon="dots-vertical" />
    </template>
    <b-dropdown-item aria-role="listitem" @click="requestFlagMessage()">
      <div class="media">
        <b-icon class="media-left" icon="flag" />
        <div class="media-content">
          <h3>Report</h3>
        </div>
      </div>
    </b-dropdown-item>
  </b-dropdown>
</template>

<script>
import { MessageFlagRepository } from "eko-sdk";

export default {
  name: "MessageOptions",
  props: ["messageModel", "lastChild"],
  methods: {
    requestFlagMessage() {
      const flagRepo = new MessageFlagRepository(this.messageModel.messageId);
      const flagged =
        this.messageModel.flagCount > 0 && this.messageModel.isFlaggedByMeCache;
      const snack = flagged
        ? "The message was unflagged!"
        : "The message was flagged!";

      if (flagged) flagRepo.unflag();
      else flagRepo.flag();

      this.$buefy.snackbar.open(snack);
    },
  },
};
</script>

<style>
.dropdown-menu {
  top: 0;
  min-width: 1rem;
}

.dropdown.is-top-left .dropdown-menu {
  top: -50%;
}

.dropdown-item {
  padding: 0.2rem 1rem !important;
}

.media {
  border-top: none;
  padding-top: 0;
}
</style>
