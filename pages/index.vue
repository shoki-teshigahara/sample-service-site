<template>
<!-- 1. response?.details が存在する場合のみ内容をレンダリング -->
<div v-if="response?.details" class="p-4 md:p-8 bg-white shadow-xl rounded-lg max-w-4xl mx-auto my-8">

    <!-- メインイメージ (オプションチェイニングで安全にアクセス) -->
    <div v-if="response.details.ext_1?.url" class="mb-6">
        <h2 class="text-2xl font-bold mb-3 text-gray-800">メイン情報</h2>
        <img 
            :src="response.details.ext_1.url" 
            alt="Main Image" 
            class="w-full rounded-lg shadow-md object-cover" 
            width="800"
        >
    </div>

    <!-- メインのテキスト情報 -->
    <div class="space-y-3 mb-8 text-gray-700">
        <p v-if="response.details.ext_2" class="text-lg">
            <strong>[Ext 2]:</strong> {{ response.details.ext_2 }}
        </p>
        <p v-if="response.details.ext_3">
            <strong>[Ext 3]:</strong> {{ response.details.ext_3 }}
        </p>
    </div>

    <!-- サブリスト (ext_4) -->
    <div v-if="response.details.ext_4?.length" class="border-t pt-6">
        <h3 class="text-xl font-semibold mb-4 text-gray-800 border-b pb-2">関連アイテム</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- nがundefinedでないことを確認するために response.details.ext_4 をチェック済み -->
            <div v-for="n in response.details.ext_4" :key="n.slag" class="bg-gray-50 p-4 rounded-md shadow-sm">
                <img 
                    :src="n.ext_4?.url" 
                    alt="Related Image" 
                    class="w-full h-40 object-cover rounded-md mb-3" 
                    width="400"
                >
                <p class="text-sm font-medium text-gray-800">{{ n.ext_3 }}</p>
                <p class="text-sm text-gray-600">{{ n.ext_5 }}</p>
                <p class="text-xs text-gray-500">{{ n.ext_6 }}</p>
            </div>
        </div>
    </div>

</div>
<!-- データロード中のメッセージ (任意) -->
<div v-else class="text-center p-10 text-gray-500">
    データをロード中です...
</div>


</template>

<script setup>
// useRuntimeConfigはNuxt 3の標準フック
const config = useRuntimeConfig();

// useFetchもNuxt 3の標準フック
const { data: response } = await useFetch(
${config.public.apiBase}/rcms-api/3/service/3,
{
credentials: 'include',
}
);
</script>