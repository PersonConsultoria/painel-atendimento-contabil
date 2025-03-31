import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { ClipboardCopy } from "lucide-react";

const responses = [
  {
    category: "Contábil",
    channel: "WhatsApp",
    question: "Preciso entregar a ECD esse ano?",
    answer:
      "Oi! Tudo bem? 😊\nVocê só precisa entregar essa obrigação se a empresa tiver algumas características específicas, como ter um faturamento mais alto ou certas mudanças internas.\nSe quiser, posso verificar isso com você rapidinho — só me diz qual o tipo da sua empresa e quanto ela faturou no ano passado."
  },
  {
    category: "Trabalhista",
    channel: "E-mail",
    question: "Minha funcionária vai sair de férias, preciso pagar tudo antes?",
    answer:
      "Olá, [nome],\nSim, o pagamento das férias precisa ser feito até no máximo o último dia útil antes do início das férias da funcionária.\nSe precisar de ajuda para calcular o valor ou emitir os recibos, posso te auxiliar com isso."
  },
  {
    category: "Fiscal",
    channel: "Chatbot",
    question: "Recebi uma notificação da Receita, e agora?",
    answer:
      "Oi! 😊 Receber um aviso da Receita pode parecer preocupante, mas muitas vezes é só uma verificação.\nVocê pode me dizer o que está escrito na notificação ou enviar uma cópia? Assim eu consigo analisar direitinho e te dizer o que fazer.\nSe preferir, posso te conectar com um atendente agora mesmo!"
  },
  {
    category: "MEI",
    channel: "WhatsApp",
    question: "Sou MEI, preciso declarar Imposto de Renda?",
    answer:
      "Oi! Boa pergunta 😊\nSe você é MEI, pode ter que declarar o Imposto de Renda da pessoa física, dependendo do quanto faturou e se teve outros rendimentos.\nQuer que eu te ajude a verificar isso?"
  }
];

export default function PainelAtendimentoPage() {
  const [search, setSearch] = useState("");

  const filteredResponses = responses.filter(
    (item) =>
      item.question.toLowerCase().includes(search.toLowerCase()) ||
      item.answer.toLowerCase().includes(search.toLowerCase()) ||
      item.category.toLowerCase().includes(search.toLowerCase()) ||
      item.channel.toLowerCase().includes(search.toLowerCase())
  );

  const copyToClipboard = async (text) => {
    try {
      await navigator.clipboard.writeText(text);
      alert("Resposta copiada com sucesso!");
    } catch (err) {
      alert("Erro ao copiar a resposta.");
    }
  };

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <h1 className="text-2xl font-bold mb-6">Painel de Atendimento Contábil</h1>
      <Input
        placeholder="Buscar por palavra-chave, canal ou categoria..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        className="mb-4"
      />
      {filteredResponses.map((item, index) => (
        <Card key={index} className="p-4 mb-4">
          <CardContent className="space-y-2">
            <div className="text-sm text-muted-foreground">
              <strong>Canal:</strong> {item.channel} | <strong>Categoria:</strong> {item.category}
            </div>
            <div>
              <strong>Pergunta:</strong> {item.question}
            </div>
            <div className="whitespace-pre-wrap">
              <strong>Resposta:</strong> {item.answer}
            </div>
            <Button
              variant="outline"
              size="sm"
              onClick={() => copyToClipboard(item.answer)}
              className="flex items-center gap-2 mt-2"
            >
              <ClipboardCopy size={16} /> Copiar resposta
            </Button>
          </CardContent>
        </Card>
      ))}
    </div>
  );
}
