<script setup lang="ts">
import Button from '@/components/ui/button/Button.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head, Link, usePage, router } from '@inertiajs/vue3';
import { Alert, AlertDescription, AlertTitle } from '@/components/ui/alert'
import { Rocket } from 'lucide-vue-next';

import {
    Table,
    TableBody,
    TableCaption,
    TableCell,
    TableHead,
    TableHeader,
    TableRow,
} from '@/components/ui/table'

interface Product {
    id: number,
    name: string,
    price: number,
    description: string,
}

interface Props {
    products: Product[];
}
//Get props from Inertia
const props = defineProps<Props>();

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Products',
        href: '/products',
    },
];

const page = usePage();

const handleDelete = (id: number) => {
    if (confirm("Do you want to delete product?")) {
        router.delete(route('products.destroy', { id }));
    }
}

</script>

<template>

    <Head title="Dashboard" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="p-4">
            <div v-if="page.props.flash?.message" class="mb-4">

                <Alert class="bg-blue-200">
                    <Rocket class="h-4 w-4" />
                    <AlertTitle>Notification!</AlertTitle>
                    <AlertDescription>
                        {{ page.props.flash.message }}
                    </AlertDescription>
                </Alert>
            </div>

            <div class="mb-10">
                <Link :href="route('products.create')"><Button>Create a Product</Button></Link>
            </div>

            <div>
                <Table>
                    <TableCaption>Product List</TableCaption>
                    <TableHeader>
                        <TableRow>
                            <TableHead class="w-[100px]">
                                ID
                            </TableHead>
                            <TableHead>Product Name</TableHead>
                            <TableHead>Price</TableHead>
                            <TableHead>Description</TableHead>
                            <TableHead class="text-center">
                                Action
                            </TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody>
                        <TableRow v-for="product in props.products" :key="product.id">

                            <TableCell>{{ product.id }}</TableCell>
                            <TableCell class="font-medium">{{ product.name }}</TableCell>
                            <TableCell>{{ product.price }}</TableCell>
                            <TableCell>{{ product.description }}</TableCell>
                            <TableCell class="text-center space-x-2">
                                <Link :href="route('products.edit', { id: product.id })"><Button
                                    class="bg-blue-500">Edit</Button></Link>
                                <Button class="bg-red-500" @click="handleDelete(product.id)">Delete</Button>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>


        </div>
    </AppLayout>
</template>
