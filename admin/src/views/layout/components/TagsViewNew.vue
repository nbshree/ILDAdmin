<template>
  <div class="tags-view">
    <div ref="tagsView"
      class="tags-outer" 
      @mousewheel="handlescroll"
      @DOMMouseScroll="handlescroll">
      <div class="tags-handle">
        <el-dropdown trigger="click" @command="handleCommand">
          <el-button type="primary">
            {{$t('tags.handle')}}<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="closeOther">{{$t('tags.closeother')}}</el-dropdown-item>
            <el-dropdown-item command="closeAll">{{$t('tags.closeall')}}</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>

      <div ref="tagsScroll"
        class="tags-inner"
        :style="{ left: tagsScrollLeft + 'px' }">
        <el-tag
          ref="tag"
          size="large"
          color="white"
          v-for="(tag, index) in visitedViews"
          :key="tag.path"
          :closable="true"
          :class="tag.name === currentViewName ? 'el-tag-active' : ''"
          @close="closeView($event, tag, index)"
          @click.native="jumpTo(tag)">
          {{$t(`route.${tag.name}`)}}
        </el-tag>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "tagsviews",
  data() {
    return {
      tagsScrollLeft: 0,
      tags: [],
      currentViewName: ""
    };
  },
  mounted() {
    this.addViewTags();

    setTimeout(() => {
      let currTagIndex = null;
      this.visitedViews.forEach((view, index) => {
        view.name === this.$route.name ? (currTagIndex = index) : null;
      });
      currTagIndex ? this.moveToView(this.$refs.tag[currTagIndex].$el) : null;
    }, 1);
  },
  computed: {
    visitedViews() {
      return this.$store.state.tagsView.visitedViews;
    }
  },
  watch: {
    $route() {
      this.addViewTags();
    }
  },
  methods: {
    moveToView(tagEle) {
      if (tagEle.offsetLeft < -this.tagsScrollLeft) {
        this.tagsScrollLeft = -tagEle.offsetLeft + 10;
      } else if (
        tagEle.offsetLeft + 10 > -this.tagsScrollLeft &&
        tagEle.offsetLeft + tagEle.offsetWidth <
          -this.tagsScrollLeft + this.$refs.tagsView.offsetWidth - 100
      ) {
        this.tagsScrollLeft = Math.min(
          0,
          this.$refs.tagsView.offsetWidth -
            100 -
            tagEle.offsetWidth -
            tagEle.offsetLeft -
            20
        );
      } else {
        this.tagsScrollLeft = -(
          tagEle.offsetLeft -
          (this.$refs.tagsView.offsetWidth - 100 - tagEle.offsetWidth) +
          15
        );
      }
    },
    addViewTags() {
      const route = this.$route;
      if (!route.name) {
        return false;
      }
      this.currentViewName = route.name;
      this.$store.dispatch("addVisitedTag", route);
    },
    async closeView(event, tagObj, tagIndex) {
      const visitedViews = await this.$store.dispatch(
        "removeVisitedTag",
        tagObj
      );
      if (this.currentActive(tagObj)) {
        const nextTag = visitedViews.slice(tagIndex)[0];
        const prevTag = visitedViews.slice(tagIndex - 1)[0];
        if (nextTag) {
          this.$router.push(nextTag.path);
          this.currentViewName = nextTag.name;
        } else if (prevTag) {
          this.$router.push(prevTag.path);
          this.currentViewName = prevTag.name;
        } else {
          this.$router.push("/");
          this.currentViewName = "dashboard";
        }
      }
    },
    currentActive(tagObj) {
      return (
        tagObj.name === this.$route.name || tagObj.path === this.$route.path
      );
    },
    jumpTo(tag) {
      this.$router.push(tag.path);
      this.currentViewName = tag.name;
    },
    async handleCommand(command) {
      const router = this.$route;
      switch (command) {
        case "closeOther":
          if (this.visitedViews.length === 1) break;
          this.$store.dispatch("closeOtherView", router);
          break;
        case "closeAll":
          await this.$store.dispatch("closeAllView");
          this.$router.push("/");
          this.currentViewName = "dashboard";
          break;
      }
    },
    getTitle(title) {
      if (this.$te(`route.${title}`)) {
        return this.$t(`route.${title}`);
      }
      return title;
    },
    handlescroll(event) {
      let type = event.type;
      let delta = 0;
      if (type === "DOMMouseScroll" || type === "mousewheel") {
        delta = event.wheelDelta ? event.wheelDelta : -(event.detail || 0) * 40;
      }
      let left = 0;
      if (delta > 0) {
        left = Math.min(0, this.tagsScrollLeft + delta);
      } else {
        if (
          this.$refs.tagsView.offsetWidth - 100 <
          this.$refs.tagsScroll.offsetWidth
        ) {
          if (
            this.tagsScrollLeft <
            -(
              this.$refs.tagsScroll.offsetWidth -
              this.$refs.tagsView.offsetWidth +
              100
            )
          ) {
            left = this.tagsScrollLeft;
          } else {
            left = Math.max(
              this.tagsScrollLeft + delta,
              this.$refs.tagsView.offsetWidth -
                this.$refs.tagsScroll.offsetWidth -
                100
            );
          }
        } else {
          this.tagsScrollLeft = 0;
        }
      }
      this.tagsScrollLeft = left;
    }
  }
};
</script>