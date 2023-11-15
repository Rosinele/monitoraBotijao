<!--Realiza a listagem dos botijoes-->

<template>
  <div class="cards">
    <va-card class="larger-padding col-span-12">
      <va-card-title>{{ t('monitora.operacoes') }}</va-card-title>
      <va-card-content>
        <va-popover
          icon="propane_tank"
          color="warning"
          message="Clique e cadastre um novo Botijao!"
          placement="right"
          open
        >
          <va-chip shadow color="primary" to="editar" @click="novoBotijao">{{
            t('monitora.botijao.botaoNovo')
          }}</va-chip>
        </va-popover>
      </va-card-content>
    </va-card>

    <va-separator />

    <div class="cards-container grid grid-cols-12 items-start gap-6 wrap">
      <template v-for="botijao in botijao2" :key="botijao._id.$oid">
        <va-card class="col-span-12 sm:col-span-6 md:col-span-3" stripe>
          <va-card-title>
            <va-avatar style="border: 2px solid AliceBlue">
              <img src="../../../../../public/tanque-de-gas.png" alt="Botijao" style="background-color: AliceBlue" />
            </va-avatar>
            <va-spacer />
            {{ botijao.nome }}
            <va-spacer />
            <va-button size="small" icon="edit" to="visualizar" @click="setBotijaoCorrente(botijao)" />
          </va-card-title>
          <va-card-content>
            <div><strong>Status:</strong> {{ botijao.ativo ? 'Ativo' : 'Inativo' }}</div>
            <div>{{ botijao.descricao }}</div>
          </va-card-content>
        </va-card>
      </template>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { computed } from 'vue'
  import { useI18n } from 'vue-i18n'
  import { useRouter } from 'vue-router'
  import { listaBotijao, IBotijao } from '../../../../stores/data-atlas'


  const router = useRouter()
  const { t } = useI18n()
  const store = listaBotijao()
  const botijao2 = computed(() => store.botijaoDTO)
  const increment = () => store.loadBotijaoList()
  const novoBotijao = () => store.novoBotijao()
  increment()
  store.loadIDDevicesList()
  /**
   * Metodo que inicia a visualização do botijao
   * @param botijao
   */
  async function setBotijaoCorrente(botijao: IBotijao) {
    const store = listaBotijao()
    await store.setBotijaoCorrente(botijao)
    await store.carregarMedicoes(botijao)
    router.push({ name: 'gasVisualizar' })
  }
</script>
