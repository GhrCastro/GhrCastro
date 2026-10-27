# Contribuições privadas no mapa 3D do perfil

O workflow [profile-3d.yml](workflows/profile-3d.yml) usa `yoshi389111/github-profile-3d-contrib`, que lê o calendário de contribuições da conta `GhrCastro` via GitHub GraphQL. O calendário inclui contribuições elegíveis em repositórios pessoais e organizações (inclusive `coderats-hub`) **quando o token tem acesso**.

## Configuração (uma vez)

1. Na conta **GhrCastro**, acesse **Settings → Developer settings → Personal access tokens**. Gere um token **classic**, com escopo `read:user` e `repo` para ler as contribuições em repositórios privados aos quais você tem acesso. Use expiração e privilégio mínimo possível; `repo` é um escopo amplo e concede mais acesso que o estritamente desejável. A API de contribuições do GitHub pode não oferecer uma alternativa fine-grained equivalente para todos os casos. **Nunca coloque o token no YAML nem no README.**
2. Caso a organização imponha SSO/SAML ou aprovação de PATs, autorize o token na `coderats-hub` segundo as políticas da organização. A organização pode bloquear o acesso ou exigir um processo de aprovação.
3. No repositório **GhrCastro/GhrCastro**, acesse **Settings → Secrets and variables → Actions → New repository secret**, nomeie-o `PROFILE_CONTRIBUTIONS_TOKEN` e cole o token.
4. Ative **Include private contributions on my profile** nas configurações do perfil do GitHub, caso ainda esteja desativado.
5. Após integrar o PR, execute **Actions → GitHub-Profile-3D-Contrib → Run workflow**, selecione `main` e confira o SVG em `profile-3d-contrib/profile-night-rainbow.svg`. A atualização diária continua ativa.

Sem o segredo, o workflow usa `github.token` e emite um aviso; assim o mapa público continua sendo gerado. O segredo é passado somente à action de geração, não à etapa de push (que usa a permissão `contents: write` do token padrão).

## Observações

- **Não conta qualquer commit automaticamente.** O mapa usa o calendário oficial do GitHub, com regras para e-mail de autoria reconhecido, branch padrão ou `gh-pages` e contribuições elegíveis; trabalho em branches não integradas pode não aparecer.
- **Não há filtro por organização:** o calendário combina todas as contribuições elegíveis visíveis para o token; `coderats-hub` não precisa ser listada.
- O gráfico público mostra **contagens e datas**, não o conteúdo privado dos repositórios. Ainda assim, considere que a densidade de contribuições privadas se torna visível a quem acessa seu README.
- Este ajuste afeta o mapa SVG gerado pelo workflow. As imagens de `github-profile-summary-cards.vercel.app` são serviços independentes e não recebem o token.
- Tokens classic com escopo `repo` podem ter acesso amplo a repositórios privados; avalie com a equipe da organização e prefira um token específico, com validade curta e revogação fácil.
