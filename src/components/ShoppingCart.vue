<script setup>
    import {useCartStore} from "@/stores/cart.js";
    import ShoppingCartItems from "@/components/ShoppingCartItems.vue";
    import Amount from "@/components/Amount.vue";
    import CouponForm from "@/components/CouponForm.vue";
    import {formatCurrency} from "../helpers/index.js";
    import {useCouponStore} from "@/stores/coupons.js";

    const cart = useCartStore()
    const coupon = useCouponStore()
</script>

<template>
    <p v-if="cart.isEmpty" class="text-xl text-center text-gray-900">El carrito esta vacio</p>

  <div v-else>
      <p class="text-4xl font-bold text-gray-900">Resumen de Venta</p>
    <ul
      role="list"
      class="mt-6 divide-y divide-gray-200"
    >
      <ShoppingCartItems
        v-for="item in cart.items"
        :key="item.id"
        :item="item"
      />
    </ul>

    <dl class="space-y-6 border-t border-gray-200 pt-6 text-sm font-medium text-gray-500">
        <Amount>
            <template #label>Subtotal: </template>
            {{formatCurrency(cart.subtotal)}}
        </Amount>
      <Amount>
        <template #label>Impuestos: </template>
        {{formatCurrency(cart.taxes)}}
      </Amount>
      <Amount v-if="coupon.isValidCoupon">
        <template #label>Descuento: </template>
        {{formatCurrency(coupon.discount)}}
      </Amount>
      <Amount>
        <template #label>Total a Pagar: </template>
        {{formatCurrency(cart.total)}}
      </Amount>
    </dl>
    <CouponForm/>

    <button
      type="button"
      class="mt-10 w-full bg-indigo-600 hover:bg-indigo-700 p-3 uppercase text-white font-bold"
      @click="cart.checkout"
    >
      Confirmar Compra
    </button>

  </div>
</template>