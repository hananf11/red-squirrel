<script lang="ts">
import { defineComponent, h, PropType, Comment, mergeProps, cloneVNode } from 'vue'
import { VNode } from '@vue/runtime-core'

export default defineComponent({
  name: 'VGroup',
  props: {
    tag: {
      type: String,
      default: 'div'
    },
    variant: {
      type: String as PropType<
        'default' | 'primary' | 'success' | 'error' | 'warning' | 'text'
      >,
      default: 'default'
    },
    size: {
      type: String as PropType<'sm' | 'md' | 'lg'>,
      default: 'md'
    },
    hollow: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    vertical: {
      type: Boolean,
      default: false
    },
    modelValue: {
      type: Object,
      default: () => ({})
    },
    modelValueMapping: {
      type: Array as PropType<'string'[]>,
      default: () => []
    }
  },
  emits: ['update:modelValue'],
  setup (props, { slots, emit }) {
    return () => {
      let offset = 0
      return h(
        props.tag,
        { class: { vertical: props.vertical, group: true } },
          slots.default?.().map((child, i) => {
            if (child.type === Comment) {
              offset++
              return child
            } else {
              const index = i - offset
              return h(child, {
                size: props.size,
                variant: child.props?.variant ?? props.variant,
                hollow: child.props?.hollow ?? props.hollow,
                disabled: props.disabled || child.props?.disabled,
                modelValue:
                  child.props?.modelValue ??
                  child.props?.['model-value'] ??
                  props.modelValue[props.modelValueMapping[index] ?? i],
                'onUpdate:modelValue':
                  child.props?.['onUpdate:modelValue'] ??
                  (typeof props.modelValue !== 'undefined' &&
                    (
                      (value?: string|number|boolean) => emit('update:modelValue', {
                        ...props.modelValue,
                        [props.modelValueMapping[index] ?? index]: value
                      })
                    )
                  ),
                class: (
                  index === 0
                    ? 'group-item-start'
                    : index === (slots.default?.().length || 1) - offset
                      ? 'group-item-end'
                      : 'group-item'
                ) + (
                  props.vertical
                    ? '-vertical'
                    : ''
                )
              })
            }
          }
          )
      )
    }
  }
})
</script>

<style scoped lang="postcss">
* {
  @apply flex;

  & > :deep(*) {
    @apply rounded-none;
    @apply flex-grow;
    &:focus {
      @apply z-10;
    }
    &:first-child {
      @apply rounded-l-md;
    }
    &:last-child {
      @apply rounded-r-md;
    }
  }

  &:not(:only-child) {
    &.group-item-start > :deep(*):last-child { @apply rounded-none; }
    &.group-item-end > :deep(*):first-child { @apply rounded-none; }
    &.group-item > :deep(*), &.group-item-vertical > :deep(*) { @apply rounded-none; }
    &.group-item-start-vertical > :deep(*) { @apply rounded-b-none; }
    &.group-item-end-vertical > :deep(*) { @apply rounded-t-none; }
  }
  & > :deep(*) {
    & .rounded-within {
      @apply rounded-none;
    }
    & .rounded-within:focus {
      @apply z-10;
    }
    &:first-child .rounded-within {
      @apply rounded-l-md;
    }
    &:last-child .rounded-within {
      @apply rounded-r-md;
    }
  }

  &.vertical {
    @apply flex-col;

    & > :deep(*) {
      @apply rounded-none;
      &:focus {
        @apply z-10;
      }
      &:first-child {
        @apply rounded-t-md;
      }
      &:last-child {
        @apply rounded-b-md;
      }
    }

    & > :deep(*) {
      & .rounded-within {
        @apply rounded-none;
      }
      & .rounded-within:focus {
        @apply z-10;
      }
      &:first-child .rounded-within {
        @apply rounded-t-md;
      }
      &:last-child .rounded-within {
        @apply rounded-b-md;
      }
    }

    &.group-item-start-vertical > :deep(*):last-child {
      @apply rounded-none;
    }

    &.group-item-end-vertical > :deep(*):first-child {
      @apply rounded-none;
    }

    &.group-item-vertical > :deep(*) {
      @apply rounded-none;
    }
  }
}
</style>
