variable "ingress" {
  type = list(object({
    from_port = number
    to_port   = number
    protocol  = string
  }))
  default = [
    { from_port = 22, to_port = 22, protocol = "TCP" },
    { from_port = 443, to_port = 443, protocol = "TCP" },
    { from_port = 80, to_port = 80, protocol = "TCP" },
    { from_port = 8080, to_port = 8080, protocol = "TCP" },
    { from_port = 9090, to_port = 9090, protocol = "TCP" },
    { from_port = 9100, to_port = 9100, protocol = "TCP" },
    { from_port = 8081, to_port = 8081, protocol = "TCP" },
    { from_port = 3000, to_port = 3000, protocol = "TCP" },
    { from_port = 9000, to_port = 9000, protocol = "TCP" },
    { from_port = 10000, to_port = 20000, protocol = "TCP" },
    { from_port = 20001, to_port = 40000, protocol = "TCP" },
    { from_port = 465, to_port = 465, protocol = "TCP" }
  ]
}

variable "egress" {
  type = list(object({
    from_port = number
    to_port   = number
    protocol  = string
  }))
  default = [
    { from_port = 22, to_port = 22, protocol = "TCP" },
    { from_port = 443, to_port = 443, protocol = "TCP" },
    { from_port = 80, to_port = 80, protocol = "TCP" },
    { from_port = 8080, to_port = 8080, protocol = "TCP" },
    { from_port = 9090, to_port = 9090, protocol = "TCP" },
    { from_port = 9100, to_port = 9100, protocol = "TCP" },
    { from_port = 8081, to_port = 8081, protocol = "TCP" },
    { from_port = 3000, to_port = 3000, protocol = "TCP" },
    { from_port = 9000, to_port = 9000, protocol = "TCP" },
    { from_port = 10000, to_port = 20000, protocol = "TCP" },
    { from_port = 20001, to_port = 40000, protocol = "TCP" },
    { from_port = 465, to_port = 465, protocol = "TCP" }
  ]
}

resource "aws_security_group" "terraform-sg" {
  name = "terraform-sg"

  dynamic "ingress" {
    iterator = port
    for_each = var.ingress
    content {
      from_port   = port.value.from_port
      to_port     = port.value.to_port
      protocol    = port.value.protocol
      cidr_blocks = [ "0.0.0.0/0" ]
    }
  }

  dynamic "egress" {
    iterator = port
    for_each = var.egress
    content {
      from_port   = port.value.from_port
      to_port     = port.value.to_port
      protocol    = port.value.protocol
      cidr_blocks = [ "0.0.0.0/0" ]
    }
  }

  tags = {
    Name = "terraform-sg"
  }
}



